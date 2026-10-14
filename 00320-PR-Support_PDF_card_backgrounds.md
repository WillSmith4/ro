# Support PDF card backgrounds

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#320](https://github.com/rooootdev/lara/pull/320)  
> **State:** 🟢 open  
> **Branch:** `SimplyRin:SimplyRin-patch-1` → `rooootdev:main`  
> **Changes:** +36 / -8 across 1 files  
> **Created:** Aug 24, 2026  

---

## Description

*Opened by [@SimplyRin](https://github.com/SimplyRin)* *on Aug 24, 2026*


### Summary
- Adds support for PDF-based card backgrounds.
- Generates PDF thumbnails using the PDF crop box while preserving aspect ratio.
- Preserves the original PDF page dimensions when replacing a PDF background.

### Details
- `cardBackground.pdf` and `cardBackgroundCombined.pdf` are now recognized.
- PDF previews are rendered from the first page.
- Replacing a PDF-backed card creates a PDF matching the original page bounds.


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (1)

- [`68d1b5c`](https://github.com/rooootdev/lara/commit/68d1b5c2728dbc6bc95fe6ddcdc14e6f0488084e) Support PDF card backgrounds — *SimplyRin* · Aug 24, 2026


---

## Changed Files (1)

- 🟡 `lara/views/tweaks/CardView.swift` (modified) — +36 / -8
  <details><summary>Diff</summary>

  ```diff
  @@ -35,7 +35,9 @@ struct CardView: View {
     private static let cardfiles = [
         "cardBackground@2x.png",
         "cardBackgroundCombined@2x.png",
-        "cardBackgroundCombined-watch@2x.png"
+        "cardBackgroundCombined-watch@2x.png",
+        "cardBackground.pdf",
+        "cardBackgroundCombined.pdf"
     ]
 
     private struct carditem: Identifiable {
@@ -393,7 +395,7 @@ struct CardView: View {
         if lower.hasSuffix(".pdf") {
             if let doc = PDFDocument(url: URL(fileURLWithPath: card.imgpath)),
                let page = doc.page(at: 0) {
-                return page.thumbnail(of: CGSize(width: 640, height: 400), for: .cropBox)
+                return pdfThumbnail(page: page)
             }
         } else if let img = UIImage(contentsOfFile: card.imgpath) {
             return img
@@ -403,7 +405,7 @@ struct CardView: View {
             if lower.hasSuffix(".pdf") {
                 if let doc = PDFDocument(data: data),
                    let page = doc.page(at: 0) {
-                    return page.thumbnail(of: CGSize(width: 640, height: 400), for: .cropBox)
+                    return pdfThumbnail(page: page)
                 }
             } else {
                 return UIImage(data: data)
@@ -412,6 +414,18 @@ struct CardView: View {
         return nil
     }
 
+    private func pdfThumbnail(page: PDFPage) -> UIImage? {
+        let bounds = page.bounds(for: .cropBox)
+        guard bounds.width > 0, bounds.height > 0 else {
+            return page.thumbnail(of: CGSize(width: 280, height: 180), for: .cropBox)
+        }
+        let targetW: CGFloat = 280
+        let targetH: CGFloat = 180
+        let scale = min(targetW / bounds.width, targetH / bounds.height)
+        let size = CGSize(width: max(bounds.width * scale, 1), height: max(bounds.height * scale, 1))
+        return page.thumbnail(of: size, for: .cropBox)
+    }
+
     private func applyreplace(card: carditem, imgdata: Data) {
         guard let image = UIImage(data: imgdata) else {
             status = "Invalid image data"
@@ -423,11 +437,7 @@ struct CardView: View {
         if lower.hasSuffix(".png") {
             payload = image.pngData()
         } else if lower.hasSuffix(".pdf") {
-            let pdf = PDFDocument()
-            if let page = PDFPage(image: image) {
-                pdf.insert(page, at: 0)
-                payload = pdf.dataRepresentation()
-            }
+            payload = makePDFPayload(image: image, originalPath: card.imgpath)
         } else {
             payload = image.pngData()
         }
@@ -447,6 +457,24 @@ struct CardView: View {
         }
     }
 
+    private func makePDFPayload(image: UIImage, originalPath: String) -> Data? {
+        var pageRect = CGRect(x: 0, y: 0, width: 886, height: 578)
+        if let doc = PDFDocument(url: URL(fileURLWithPath: originalPath)),
+           let page = doc.page(at: 0) {
+            let bounds = page.bounds(for: .cropBox)
+            if bounds.width > 0, bounds.height > 0 {
+                pageRect = CGRect(x: 0, y: 0, width: bounds.width, height: bounds.height)
+            }
+        }
+
+        let renderer = UIGraphicsPDFRenderer(bounds: pageRect)
+        let data = renderer.pdfData { ctx in
+            ctx.beginPage()
+            image.draw(in: pageRect)
+        }
+        return data
+    }
+
     private func backupifneeded(card: carditem) {
         let backuppath = card.imgpath + ".backup"
         let fm = FileManager.default
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 68d1b5c: Support PDF card backgrounds


---

*Exported on 10/14/2026, 3:13:05 PM from [https://github.com/rooootdev/lara/pull/320](https://github.com/rooootdev/lara/pull/320)*
