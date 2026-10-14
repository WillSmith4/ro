# Cylinder tweak

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#235](https://github.com/rooootdev/lara/pull/235)  
> **State:** 🔴 closed  
> **Branch:** `notthemystery:main` → `rooootdev:main`  
> **Changes:** +3161 / -0 across 57 files  
> **Created:** May 8, 2026  
> **Updated:** May 8, 2026  
> **Closed:** May 8, 2026  

---

## Description

*Opened by [@notthemystery](https://github.com/notthemystery)* *on May 8, 2026*


I added support for the cylinder springboard tweak in lara


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (3)

- [`224cbde`](https://github.com/rooootdev/lara/commit/224cbdeab6dd9faf4a6f0cea963ac35a789ccb14) Update TweaksView.swift — *The Mystery* · May 8, 2026
- [`85b7ba0`](https://github.com/rooootdev/lara/commit/85b7ba073b32b98f41ab0675c53b4108f0094540) Add files via upload — *The Mystery* · May 8, 2026
- [`31cbd3a`](https://github.com/rooootdev/lara/commit/31cbd3a3486e76d8c8f5ed6a62eb8502f8723333) Add files via upload — *The Mystery* · May 8, 2026


---

## Changed Files (57)

- 🟢 `Cylinder/Cylinder.plist` (added) — +1 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1 @@
+{ Filter = { Bundles = ( "com.apple.springboard" ); }; }
  ```

  </details>
- 🟢 `Cylinder/Defines.h` (added) — +44 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,44 @@
+#import <UIKit/UIKit.h>
+#import <os/log.h>
+#import <substrate.h>
+
+#define CLLog(format, ...) os_log(OS_LOG_DEFAULT, "Cylinder: %@", [NSString stringWithFormat: format, ## __VA_ARGS__])
+#define Log CLLog
+
+#define SCREEN_SIZE UIScreen.mainScreen.bounds.size
+
+#define PrefsEffectKey        @"effect"
+#define PrefsEffectDirKey  @"effectSelector"
+#define PrefsFormulaKey @"formula"
+#define PrefsSelectedFormulaKey @"selectedFormula"
+#define PrefsCarrierTextKey  @"carrierText"
+#define PrefsUseTextKey      @"useText"
+#define PrefsEnabledKey      @"enabled"
+#define PrefsRandomizedKey      @"randomized"
+#define PrefsOldMethodKey    @"useOldMethod"
+
+#define PrefsPackKey         @"pack"
+#define PrefsBrokenKey       @"brokenEffects"
+
+#define DEFAULT_EFFECT @"Cube (inside)"
+#define DEFAULT_SELECTOR @"cubeInside"
+
+#ifndef MAIN_BUNDLE
+#define MAIN_BUNDLE ([NSBundle bundleForClass:NSClassFromString(@"CylinderSettingsListController")])
+#endif
+#define LOCALIZE(KEY, DEFAULT) [MAIN_BUNDLE localizedStringForKey:KEY value:DEFAULT table:@"CylinderSettings"]
+#define SYSTEM_LOCALIZE(KEY) [[NSBundle bundleWithIdentifier:@"com.apple.UIKit"] localizedStringForKey:KEY value:@"" table:nil]
+
+#define IN_SPRINGBOARD()     ([[[NSBundle mainBundle] bundleIdentifier] isEqualToString:@"com.apple.springboard"])
+#define PREFS_PATH           @"/var/mobile/Library/Preferences/com.ryannair05.cylinderremade.plist"
+#define RETINIZE(r)          [r stringByAppendingString:@"@2x"]
+
+#define kCylinderSettingsChanged         @"com.ryannair05.cylinderremade/settingsChanged"
+#define kCylinderSettingsRefreshSettings @"com.ryannair05.cylinderremade/refreshSettings"
+
+#define BUNDLE_PATH @"/Library/PreferenceBundles/CylinderSettings.bundle/"
+
+#define DEFAULT_EFFECTS      [NSArray arrayWithObjects:[NSDictionary dictionaryWithObjectsAndKeys:DEFAULT_EFFECT, PrefsEffectKey, DEFAULT_SELECTOR, PrefsEffectDirKey, nil], nil]
+#define DEFAULT_FORMULAS    [NSDictionary dictionary]
+#define DefaultPrefs         [NSMutableDictionary dictionaryWithObjectsAndKeys: DEFAULT_EFFECTS, PrefsEffectKey, DEFAULT_FORMULAS, PrefsFormulaKey, [NSNumber numberWithBool:YES], PrefsEnabledKey, [NSNumber numberWithBool:false], PrefsRandomizedKey, nil]
+
  ```

  </details>
- 🟢 `Cylinder/Makefile` (added) — +20 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,20 @@
+THEOS_DEVICE_IP = 192.168.1.253
+
+FINALPACKAGE = 1
+
+TARGET := iphone:clang:latest:14.0
+
+export ADDITIONAL_CFLAGS = -DTHEOS_LEAN_AND_MEAN -fobjc-arc
+
+include $(THEOS)/makefiles/common.mk
+
+TWEAK_NAME = Cylinder
+Cylinder_FILES = $(wildcard tweak/*.swift) tweak/tweak.x tweak/CALayer+Cylinder.m
+Cylinder_SWIFT_BRIDGING_HEADER = tweak/Cylinder-Bridging-Header.h
+
+include $(THEOS_MAKE_PATH)/tweak.mk
+
+after-install::
+	install.exec "killall -9 SpringBoard"
+SUBPROJECTS += settings
+include $(THEOS_MAKE_PATH)/aggregate.mk
  ```

  </details>
- 🟢 `Cylinder/control` (added) — +11 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,11 @@
+Package: com.ryannair05.cylinderremade
+Name: Cylinder Remade
+Architecture: iphoneos-arm
+Depends: firmware (>= 14.0), mobilesubstrate (>= 0.9.6011), preferenceloader
+Icon: file:///Library/PreferenceBundles/CylinderSettings.bundle/Icon@3x.png
+Description: Make your icons dance
+Maintainer: Ryan Nair
+Author: Ryan Nair
+Section: Tweaks
+Conflicts: com.ryannair05.cylinder
+Version: 0.0.4-beta
\ No newline at end of file
  ```

  </details>
- 🟢 `Cylinder/settings/CLAlignedTableViewCell.h` (added) — +5 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,5 @@
+#import <UIKit/UIKit.h>
+
+@interface CLAlignedTableViewCell : UITableViewCell
+@property (nonatomic, retain) UILabel *numberLabel;
+@end
  ```

  </details>
- 🟢 `Cylinder/settings/CLAlignedTableViewCell.m` (added) — +58 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,58 @@
+#import "CLAlignedTableViewCell.h"
+
+#define IMAGE_PADDING 10
+#define NUMBER_WIDTH 44
+#define NUMBER_PADDING 10
+
+@implementation CLAlignedTableViewCell
+@synthesize numberLabel=_numberLabel;
+
+-(id)initWithStyle:(UITableViewCellStyle)style reuseIdentifier:(NSString *)reuseIdentifier
+{
+    if(self == [super initWithStyle:style reuseIdentifier:reuseIdentifier])
+    {
+        _numberLabel = [[UILabel alloc] init];
+        _numberLabel.font = [_numberLabel.font fontWithSize:16];
+        _numberLabel.textColor = [UIColor systemBlueColor];
+    }
+    return self;
+}
+
+- (void) layoutSubviews
+{
+    [super layoutSubviews];
+    CGSize cvs = self.contentView.frame.size;
+    CGFloat width = 44; //60;
+    self.imageView.frame = CGRectMake(IMAGE_PADDING, IMAGE_PADDING, width - IMAGE_PADDING*2, cvs.height-1 - IMAGE_PADDING*2);
+    //self.imageView.contentMode = UIViewContentModeCenter;//|UIViewContentModeScaleAspectFit;
+
+
+    NSAttributedString *text = [[NSAttributedString alloc] initWithString:self.numberLabel.text  attributes:@{NSFontAttributeName:self.numberLabel.font}];
+
+    CGSize size = 
+    // _numberLabel ? [self.numberLabel.text sizeWithFont:self.numberLabel.font
+    //                                     constrainedToSize:CGSizeMake(NUMBER_WIDTH,self.textLabel.frame.size.height)
+    //                                         lineBreakMode:self.numberLabel.lineBreakMode] : CGSizeZero;
+
+    _numberLabel ? [text boundingRectWithSize:CGSizeMake(NUMBER_WIDTH,self.textLabel.frame.size.height) options:NSStringDrawingUsesLineFragmentOrigin context:nil].size : CGSizeZero;
+
+    size.width += NUMBER_PADDING;
+
+    self.numberLabel.frame = CGRectMake(
+            cvs.width - size.width,
+            self.textLabel.frame.origin.y,
+            size.width,
+            self.textLabel.frame.size.height);
+
+    self.textLabel.frame = CGRectMake(width,
+                              self.textLabel.frame.origin.y,
+                              cvs.width - width*2 - size.width,
+                              self.textLabel.frame.size.height);
+
+    self.detailTextLabel.frame = CGRectMake(width,
+                       self.detailTextLabel.frame.origin.y,
+                       cvs.width - width*2,
+                       self.detailTextLabel.frame.size.height);
+    [self addSubview:self.numberLabel];
+}
+@end
  ```

  </details>
- 🟢 `Cylinder/settings/CLDonateController.h` (added) — +11 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,11 @@
+#import <Preferences/PSViewController.h>
+#import <AVFoundation/AVFoundation.h>
+
+@interface CLDonateController : PSViewController <UITableViewDelegate, UITableViewDataSource>
+{
+    UITableView *_tableView;
+    AVAudioPlayer *_player;
+}
+// -(id)initForContentSize:(CGSize)size;
+-(id)view;
+@end 
  ```

  </details>
- 🟢 `Cylinder/settings/CLDonateController.m` (added) — +150 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,150 @@
+#import "../Defines.h"
+#import "CLDonateController.h"
+#include <objc/runtime.h>
+
+#define TEXT_SECTION 0
+#define PATREON_SECTION 1
+#define BITCOIN_SECTION 2
+#define BITCOIN_ADDRESS @"177JwbKv8msAQPVk8azEKMCuNBWHJbs1XT"
+#define PATREON_URL @"https://www.patreon.com/user?u=20478714"
+
+@interface UITableView (Private)
+- (NSArray *) indexPathsForSelectedRows;
+@property(nonatomic) BOOL allowsMultipleSelectionDuringEditing;
+@end
+
+@implementation CLDonateController
+
+- (instancetype)init
+{
+	if (self = [super init])
+    {
+		_tableView = [[UITableView alloc] initWithFrame:CGRectMake(0, 0, self.view.frame.size.width, self.view.frame.size.height) style:UITableViewStyleGrouped];
+		[_tableView setDataSource:self];
+		[_tableView setDelegate:self];
+		
+		if ([self respondsToSelector:@selector(setView:)])
+			[self performSelectorOnMainThread:@selector(setView:) withObject:_tableView waitUntilDone:YES];
+
+        UILabel *titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(0,0,10,10)];
+        titleLabel.font = [UIFont boldSystemFontOfSize:17];
+        titleLabel.text = @"Donate";
+        titleLabel.translatesAutoresizingMaskIntoConstraints = NO;
+        self.navigationItem.titleView = [UIView new];
+        [self.navigationItem.titleView addSubview:titleLabel];
+
+        [NSLayoutConstraint activateConstraints:@[
+            [titleLabel.topAnchor constraintEqualToAnchor:self.navigationItem.titleView.topAnchor],
+            [titleLabel.leadingAnchor constraintEqualToAnchor:self.navigationItem.titleView.leadingAnchor],
+            [titleLabel.trailingAnchor constraintEqualToAnchor:self.navigationItem.titleView.trailingAnchor],
+            [titleLabel.bottomAnchor constraintEqualToAnchor:self.navigationItem.titleView.bottomAnchor],
+        ]];
+
+        NSError *sessionError = nil;
+        [[AVAudioSession sharedInstance] setCategory:AVAudioSessionCategoryAmbient error:&sessionError];
+        [[AVAudioSession sharedInstance] setActive:true error:&sessionError];
+
+        NSError *error;
+        _player = [[AVAudioPlayer alloc] initWithContentsOfURL:[NSURL URLWithString:BUNDLE_PATH "iloveyou.mp3"] error:&error];
+        if(!error)
+        {
+            [_player prepareToPlay];
+        }
+	}
+	return self;
+}
+
+- (void)viewWillAppear:(BOOL)animated
+{
+    [super viewWillAppear:animated];
+}
+
+- (void)dealloc
+{
+    [_player stop];
+}
+
+- (id)view
+{
+    return _tableView;
+}
+
+/* UITableViewDelegate / UITableViewDataSource Methods {{{ */
+- (NSInteger) numberOfSectionsInTableView:(UITableView *)tableView
+{
+    return 2;
+}
+
+- (id) tableView:(UITableView *)tableView titleForHeaderInSection:(NSInteger)section
+{
+    switch(section)
+    {
+        case BITCOIN_SECTION:
+            return @"Bitcoin (tap to copy address)";
+        case PATREON_SECTION:
+            return @"Patreon";
+    }
+    return nil;
+}
+
+- (NSInteger) tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
+{
+    return 1;
+}
+
+-(id)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
+{
+    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"yeah"];
+    if(!cell)
+    {
+        cell = [UITableViewCell.alloc initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"EffectCell"];
+        cell.textLabel.numberOfLines = 0;
+        cell.selectionStyle = UITableViewCellSelectionStyleBlue;
+    }
+    //cell.selectionStyle = UITableViewCellSelectionStyleBlue;
+    switch(indexPath.section)
+    {
+        case TEXT_SECTION:
+            cell.textLabel.text = [NSString stringWithFormat:@"%@%@%@", @"\u2764\u2764\u2764\u2764 ", LOCALIZE(@"THANK_YOU", @"Thank you"), @" \u2764\u2764\u2764\u2764"];
+        break;
+        case BITCOIN_SECTION:
+            cell.textLabel.text = BITCOIN_ADDRESS;
+        break;
+        case PATREON_SECTION:
+            cell.textLabel.text = LOCALIZE(@"PAYPAL", @"Tap here to go to Safari and donate to via Patreon");
+        break;
+    }
+    return cell;
+}
+
+- (void) tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
+{
+    [tableView deselectRowAtIndexPath:indexPath animated:true];
+    if(indexPath.section == BITCOIN_SECTION)
+    {
+        UIPasteboard.generalPasteboard.string = BITCOIN_ADDRESS;
+
+        UIAlertController * alert = [UIAlertController alertControllerWithTitle:@"Address copied!" message:nil preferredStyle:UIAlertControllerStyleAlert];
+        UIAlertAction* okayButton = [UIAlertAction actionWithTitle:@"Okay" style:UIAlertActionStyleDefault handler:^(UIAlertAction * action) {}];
+        [alert addAction:okayButton];
+        [self presentViewController:alert animated:YES completion:nil];
+
+    }
+    else if(indexPath.section == PATREON_SECTION)
+    {
+        [[UIApplication sharedApplication] openURL:[NSURL URLWithString:PATREON_URL] options:@{} completionHandler:nil];
+    }
+    else
+    {
+        _player.currentTime = 0;
+        [_player prepareToPlay];
+        [_player play];
+    }
+}
+
+- (UITableViewCellEditingStyle)tableView:(UITableView*)tableView editingStyleForRowAtIndexPath:(NSIndexPath*)indexPath
+{
+    return UITableViewCellEditingStyleNone;
+}
+
+@end
  ```

  </details>
- 🟢 `Cylinder/settings/CLEffect.h` (added) — +33 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,33 @@
+/*
+Copyright (C) 2014 Reed Weichler
+
+This file is part of Cylinder.
+
+Cylinder is free software: you can redistribute it and/or modify
+it under the terms of the GNU General Public License as published by
+the Free Software Foundation, either version 3 of the License, or
+(at your option) any later version.
+
+Cylinder is distributed in the hope that it will be useful,
+but WITHOUT ANY WARRANTY; without even the implied warranty of
+MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
+GNU General Public License for more details.
+
+You should have received a copy of the GNU General Public License
+along with Cylinder.  If not, see <http://www.gnu.org/licenses/>.
+*/
+
+#import <UIKit/UIKit.h>
+#import "CLAlignedTableViewCell.h"
+
+@interface CLEffect : NSObject
+
+@property (nonatomic, assign) CLAlignedTableViewCell *cell;
+@property (nonatomic, copy) NSString *name;
+@property (nonatomic, copy) NSString *selectorName;
+@property (nonatomic, copy) NSString *developer;
+@property (nonatomic, assign, getter=isSelected) BOOL selected;
+
+- (id)initWithName:(NSString*)name selectorName:(NSString *)selectorName developer:(NSString *)developer;
+
+@end
  ```

  </details>
- 🟢 `Cylinder/settings/CLEffect.m` (added) — +16 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,16 @@
+#import "CLEffect.h"
+#import "../Defines.h"
+
+@implementation CLEffect
+@synthesize name=_name, selectorName=_selectorName, developer=_developer, selected=_selected, cell=_cell;
+
+- (id)initWithName:(NSString*)name selectorName:(NSString *)selectorName developer:(NSString *)developer  {
+	if (self = [super init]) {
+		self.name = name;
+		self.selectorName = selectorName;
+		self.developer = developer;
+	}
+	return self;
+}
+
+@end
  ```

  </details>
- 🟢 `Cylinder/settings/CLEffectsController.h` (added) — +14 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,14 @@
+#import <Preferences/PSViewController.h>
+#import "CLEffect.h"
+
+@interface CLEffectsController : PSViewController <UITableViewDelegate, UITableViewDataSource> {
+	UITableView *_tableView;
+    BOOL _initialized;
+}
+@property (nonatomic, retain) NSMutableArray *effects;
+@property (nonatomic, retain) NSMutableArray *selectedEffects;
+@property (nonatomic, retain) UILabel *titleLabel;
+// + (void)load;
+- (id)view;
+- (void)refreshList;
+@end 
  ```

  </details>
- 🟢 `Cylinder/settings/CLEffectsController.m` (added) — +228 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,228 @@
+#import "../Defines.h"
+#import "CLEffectsController.h"
+#import "CylinderSettings.h"
+
+#import "CLAlignedTableViewCell.h"
+
+static CLEffectsController *sharedController = nil;
+
+@interface PSViewController(Private)
+-(void)viewWillAppear:(BOOL)animated;
+@end
+
+@implementation CLEffectsController
+@synthesize effects = _effects, selectedEffects=_selectedEffects;
+
+- (instancetype)init
+{
+	if (self = [super init])
+    {
+        _tableView = [[UITableView alloc] initWithFrame:CGRectMake(0, 0, self.view.frame.size.width, self.view.frame.size.height) style:UITableViewStyleGrouped];
+        _tableView.dataSource = self;
+        _tableView.delegate = self;
+        // _tableView.editing = false;
+        // _tableView.allowsSelection = true;
+
+        // _tableView.allowsMultipleSelection = false;
+        // _tableView.allowsSelectionDuringEditing = true;
+        // _tableView.allowsMultipleSelectionDuringEditing = true;
+    
+		if ([self respondsToSelector:@selector(setView:)])
+			[self performSelectorOnMainThread:@selector(setView:) withObject:_tableView waitUntilDone:YES];
+
+        self.titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(0,0,10,10)];
+        self.titleLabel.font = [UIFont boldSystemFontOfSize:17];
+        self.titleLabel.text = @"Effects";
+        self.titleLabel.translatesAutoresizingMaskIntoConstraints = NO;
+        self.navigationItem.titleView = [UIView new];
+        [self.navigationItem.titleView addSubview:self.titleLabel];
+
+        [NSLayoutConstraint activateConstraints:@[
+            [self.titleLabel.topAnchor constraintEqualToAnchor:self.navigationItem.titleView.topAnchor],
+            [self.titleLabel.leadingAnchor constraintEqualToAnchor:self.navigationItem.titleView.leadingAnchor],
+            [self.titleLabel.trailingAnchor constraintEqualToAnchor:self.navigationItem.titleView.trailingAnchor],
+            [self.titleLabel.bottomAnchor constraintEqualToAnchor:self.navigationItem.titleView.bottomAnchor],
+        ]];
+
+        self.navigationItem.rightBarButtonItem = [[UIBarButtonItem alloc] initWithTitle:LOCALIZE(@"RESET_EFFECTS", @"Clear") style:UIBarButtonItemStylePlain target:self action:@selector(clear:)];
+
+	}
+    sharedController = self;
+	return self;
+}
+
+- (NSString *)tableView:(UITableView *)tableView titleForFooterInSection:(NSInteger)section {
+    if (section == 0) {
+        return @"WARNING: combining certain 3D effects may cause lag";
+    }
+    return nil;
+}
+
+- (void)addEffectsFromDirectory:(NSString *)directory
+{
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Ant Lines (Horizontal)" selectorName:@"horizontalAntLines" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Ant Lines (Vertical)" selectorName:@"verticalAntLines" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Backwards" selectorName:@"backwards" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Card (Horizontal)" selectorName:@"cardHorizontal" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Card (Vertical)" selectorName:@"cardVertical" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Checkerboard Scatter" selectorName:@"scatter" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Chomp" selectorName:@"chomp" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Cube Inside" selectorName:@"cubeInside" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Cube Outside" selectorName:@"cubeOutside" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Double Door" selectorName:@"doubleDoor" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Helix" selectorName:@"doubleHelix" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Hinge" selectorName:@"hinge" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Hyperspace" selectorName:@"hyperspace" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Icon Collection" selectorName:@"iconCollection" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Left Stairs" selectorName:@"leftStairs" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Right Stairs" selectorName:@"rightStairs" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Page Fade" selectorName:@"pageFade" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Page Flip" selectorName:@"pageFlip" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Page Twist" selectorName:@"pageTwist" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Psychospiral" selectorName:@"psychospiral" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Shrink" selectorName:@"shrink" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Spin" selectorName:@"spin" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Suck" selectorName:@"suck" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Vertical Scrolling" selectorName:@"verticalScrolling" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Vortex" selectorName:@"vortex" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Wave" selectorName:@"wave" developer:@"Ryan Nair"]];
+    [self.effects addObject:[[CLEffect alloc] initWithName:@"Wheel" selectorName:@"wheel" developer:@"Ryan Nair"]];
+    
+    // if(effect)
+        // [self.effects addObject:effect];
+
+    [self.effects sortUsingComparator:^NSComparisonResult(CLEffect *effect1, CLEffect *effect2)
+    {
+        return [effect1.name compare:effect2.name];
+    }];
+}
+
+-(CLEffect *)effectWithName:(NSString *)name
+{
+    if(!name) return nil;
+
+    for(CLEffect *effect in self.effects)
+    {
+        if([effect.name isEqualToString:name])
+        {
+            return effect;
+        }
+    }
+    return nil;
+}
+
+- (void)refreshList
+{
+    self.effects = [NSMutableArray array];
+    CylinderSettingsListController *ctrl = (CylinderSettingsListController*)self.parentController;
+    [self addEffectsFromDirectory:nil];
+
+    NSArray *effects = [ctrl.settings objectForKey:PrefsEffectKey];
+    self.selectedEffects = [NSMutableArray array];
+
+    for(NSDictionary *dict in effects)
+    {
+        NSString *name = [dict objectForKey:PrefsEffectKey];
+        CLEffect *effect = [self effectWithName:name];
+        effect.selected = true;
+        if(effect)
+            [self.selectedEffects addObject:effect];
+    }
+}
+
+- (void)clear:(id)sender
+{
+    for(CLEffect *effect in self.selectedEffects)
+    {
+        effect.selected = false;
+    }
+
+    self.selectedEffects = [NSMutableArray array];
+    [_tableView reloadData];
+
+    [self updateSettings];
+}
+
+- (void)viewWillAppear:(BOOL)animated
+{
+    if(!_initialized)
+    {
+        [self refreshList];
+        _initialized = true;
+    }
+    [super viewWillAppear:animated];
+
+}
+
+- (void)dealloc
+{
+    sharedController = nil;
+}
+
+- (id)view
+{
+    return _tableView;
+}
+
+- (NSInteger) tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
+{
+    return self.effects.count;
+
+}
+
+-(id)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
+{
+    CLAlignedTableViewCell *cell = (CLAlignedTableViewCell*)[tableView dequeueReusableCellWithIdentifier:@"EffectCell"];
+    if (!cell)
+    {
+        cell = [CLAlignedTableViewCell.alloc initWithStyle:UITableViewCellStyleSubtitle reuseIdentifier:@"EffectCell"];
+        cell.textLabel.adjustsFontSizeToFitWidth = true;
+        cell.accessoryType = UITableViewCellAccessoryNone;
+    }
+
+    CLEffect *effect = [self.effects objectAtIndex:indexPath.row];
+    effect.cell = cell;
+
+    cell.textLabel.text = effect.name;
+    cell.detailTextLabel.text = effect.developer;
+
+    cell.numberLabel.text = effect.selected ? [NSString stringWithFormat:@"%lu", ([self.selectedEffects indexOfObject:effect] + 1)] : @"";
+
+    return cell;
+}
+
+- (void) tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
+{
+    if (!tableView.isEditing)
+    {
+        // deselect old one
+        [tableView deselectRowAtIndexPath:indexPath animated:true];
+
+        CLEffect *effect = [self.effects objectAtIndex:indexPath.row];
+        effect.selected = !effect.selected;
+
+        if(effect.selected)
+        {
+            [self.selectedEffects addObject:effect];
+            CLEffect *e = [self.selectedEffects objectAtIndex: self.selectedEffects.count-1];
+            CLAlignedTableViewCell *cell = (CLAlignedTableViewCell *)e.cell;
+            cell.numberLabel.text = [NSString stringWithFormat:@"%lu",  self.selectedEffects.count];
+        }
+        else
+        {
+            effect.cell.numberLabel.text = @"";
+            [self.selectedEffects removeObject:effect];
+        }
+
+        [self updateSettings];
+    }
+}
+
+-(void)updateSettings
+{
+    // make the title changes
+    CylinderSettingsListController *ctrl = (CylinderSettingsListController*)self.parentController;
+    ctrl.selectedEffects = self.selectedEffects;
+}
+
+@end
\ No newline at end of file
  ```

  </details>
- 🟢 `Cylinder/settings/CLFormulasController.h` (added) — +16 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,16 @@
+#import <Preferences/PSViewController.h>
+#import "CylinderSettings.h"
+
+@interface CLFormulasController : PSViewController <UITableViewDelegate, UITableViewDataSource, UIAlertViewDelegate> {
+	UITableView *_tableView;
+	NSMutableDictionary *_formulass;
+    NSString *_selectedFormula;
+    BOOL _initialized;
+}
+@property (nonatomic, retain) NSMutableDictionary *formulas;
+@property (nonatomic, retain) NSString *selectedFormula;
+@property (nonatomic, strong) UIBarButtonItem *editButton;
+// + (void)load;
+- (id)view;
+- (void)refreshList;
+@end 
  ```

  </details>
- 🟢 `Cylinder/settings/CLFormulasController.m` (added) — +330 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,330 @@
+#import "../Defines.h"
+#import "CLFormulasController.h"
+
+#define ADD_SECTION 0
+#define FORMULA_SECTION 1
+
+
+@interface PSViewController(Private)
+-(void)viewWillAppear:(BOOL)animated;
+-(CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath;
+@end
+
+
+@implementation CLFormulasController
+@synthesize formulas=_formulas,selectedFormula=_selectedFormula, editButton=_editButton;
+
+- (instancetype)init
+{
+	if (self = [super init])
+    {
+		_tableView = [[UITableView alloc] initWithFrame:CGRectMake(0, 0, self.view.frame.size.width, self.view.frame.size.height) style:UITableViewStyleGrouped];
+        _tableView.dataSource = self;
+        _tableView.delegate = self;
+        //_tableView.allowsSelection = true;
+        //_tableView.allowsSelectionDuringEditing = true;
+
+        /*
+			[_tableView setAllowsMultipleSelection:NO];
+			[_tableView setAllowsSelectionDuringEditing:YES];
+			[_tableView setAllowsMultipleSelectionDuringEditing:YES];
+        */
+
+		if ([self respondsToSelector:@selector(setView:)])
+			[self performSelectorOnMainThread:@selector(setView:) withObject:_tableView waitUntilDone:YES];	
+
+        UILabel *titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(0,0,10,10)];
+        titleLabel.font = [UIFont boldSystemFontOfSize:17];
+        titleLabel.text = @"Formulas";
+        titleLabel.translatesAutoresizingMaskIntoConstraints = NO;
+        self.navigationItem.titleView = [UIView new];
+        [self.navigationItem.titleView addSubview:titleLabel];
+
+        [NSLayoutConstraint activateConstraints:@[
+            [titleLabel.topAnchor constraintEqualToAnchor:self.navigationItem.titleView.topAnchor],
+            [titleLabel.leadingAnchor constraintEqualToAnchor:self.navigationItem.titleView.leadingAnchor],
+            [titleLabel.trailingAnchor constraintEqualToAnchor:self.navigationItem.titleView.trailingAnchor],
+            [titleLabel.bottomAnchor constraintEqualToAnchor:self.navigationItem.titleView.bottomAnchor],
+        ]];
+
+        self.editButton = [[UIBarButtonItem alloc] initWithTitle:@"Edit" style:UIBarButtonItemStylePlain target:self action:@selector(editButtonPressed:)];
+
+        self.navigationItem.rightBarButtonItem = self.editButton;
+	}
+	return self;
+}
+
+- (void)refreshList
+{
+    CylinderSettingsListController *ctrl = (CylinderSettingsListController*)self.parentController;
+
+    NSDictionary *formulas = [ctrl.settings objectForKey:PrefsFormulaKey];
+    if(!formulas || ![formulas isKindOfClass:NSDictionary.class])
+    {
+        self.formulas = [NSMutableDictionary dictionary];
+    }
+    else
+    {
+        self.formulas = formulas.mutableCopy;
+        /*
+        self.formulas = [NSMutableDictionary dictionaryWithCapacity:formulas.count];
+        for(NSString *key in formulas)
+        {
+            NSArray *effectDicts = [formulas objectForKey:key];
+            NSMutableArray *effects = [NSMutableArray arrayWithCapacity:effectDicts.count];
+            for(NSDictionary *effectDict in effectDicts)
+            {
+                NSString *dir = [effectDict objectForKey:PrefsEffectDirKey];
+                NSString *name = [effectDict objectForKey:PrefsEffectKey];
+
+                if(dir && name)
+                {
+                    NSString *path = [[kEffectsDirectory stringByAppendingPathComponent:dir] stringByAppendingPathComponent:name];
+                    CLEffect *effect = [CLEffect effectWithPath:path];
+                    if(effect)
+                        [effects addObject:effectDict];
+                }
+            }
+            [self.formulas setObject:effects forKey:key];
+        }
+        */
+    }
+
+    self.selectedFormula = [ctrl.settings objectForKey:PrefsSelectedFormulaKey];
+}
+
+-(void)showAlertWithText:(NSString *)text
+{
+    UIAlertController *alert = [UIAlertController alertControllerWithTitle: nil message: text preferredStyle:UIAlertControllerStyleAlert];
+
+    UIAlertAction *create = [UIAlertAction actionWithTitle: LOCALIZE(@"CREATE_FORMULA", @"Create Formula") style: UIAlertActionStyleDefault handler:^(UIAlertAction *action){
+        NSString *name = alert.textFields.firstObject.text;
+                    
+        if(name.length == 0)
+        {
+            [self showAlertWithText:@"You didn't type anything."];
+        }
+        else if([self.formulas objectForKey:name])
+        {
+            UIAlertController *sameNameAlert = [UIAlertController alertControllerWithTitle:nil message:LOCALIZE(@"FORMULA_ALREADY_EXISTS", @"A formula with that name already exists.") preferredStyle:UIAlertControllerStyleAlert];
+            UIAlertAction *overWrite = [UIAlertAction actionWithTitle: LOCALIZE(@"OVERWRITE_IT", @"Overwrite it") style: UIAlertActionStyleDestructive handler:^(UIAlertAction *action){
+                NSString *_theFormulaName = alert.textFields.firstObject.text;
+                if(name.length == 0)
+                {
+                    [self showAlertWithText:@"You didn't type anything."];
+                }
+                else {
+                    [self createFormulaWithName:_theFormulaName];
+                    _theFormulaName = nil;
+                }
+            }];
+
+            UIAlertAction *cancelOverwrite = [UIAlertAction actionWithTitle:LOCALIZE(@"CANCEL", @"Cancel") style:UIAlertActionStyleCancel handler: nil];
+
+            [sameNameAlert addAction:cancelOverwrite];
+            [sameNameAlert addAction:overWrite];
+            [self presentViewController:sameNameAlert animated:YES completion:nil];
+        }
+        else
+        {
+            [self createFormulaWithName:name];
+        }
+    }];
+
+    UIAlertAction *cancel = [UIAlertAction actionWithTitle:LOCALIZE(@"CANCEL", @"Cancel") style:UIAlertActionStyleCancel handler: nil];
+
+    [alert addTextFieldWithConfigurationHandler:^(UITextField *textField) {
+        textField.placeholder = LOCALIZE(@"FORMULA_NAME", @"Formula name");
+    }];
+
+    [alert addAction:create];
+    [alert addAction:cancel];
+
+    [self presentViewController:alert animated:YES completion:nil];
+}
+
+-(void)createFormulaWithName:(NSString *)name
+{
+    CylinderSettingsListController *ctrl = (CylinderSettingsListController*)self.parentController;
+    NSArray *effects = [ctrl.settings objectForKey:PrefsEffectKey];
+
+    if(!effects)
+    {
+        [self showAlertWithText:@"IT DIDN'T WORK!"];
+        return;
+    }
+
+    [self.formulas setObject:effects forKey:name];
+    self.selectedFormula = name;
+    [self updateSettings];
+    [_tableView reloadData];
+}
+
+- (void)editButtonPressed:(id)sender
+{
+    if(_tableView.editing)
+    {
+        [_tableView setEditing:false animated:true];
+        self.editButton.title = @"Edit";
+    }
+    else
+    {
+        [_tableView setEditing:true animated:true];
+        self.editButton.title = @"Done";
+    }
+
+}
+
+- (void)viewWillAppear:(BOOL)animated
+{
+    if(!_initialized)
+    {
+        [self refreshList];
+        _initialized = true;
+    }
+    [super viewWillAppear:animated];
+    //_tableView.editing = true;
+
+}
+
+- (id)view
+{
+    return _tableView;
+}
+
+/* UITableViewDelegate / UITableViewDataSource Methods {{{ */
+- (NSInteger) numberOfSectionsInTableView:(UITableView *)tableView
+{
+    return 2;
+}
+
+-(NSString *)keyForIndex:(int)index
+{
+    int i = 0;
+    for(NSString *key in self.formulas)
+    {
+        if(i == index) return key;
+        i++;
+    }
+    return nil;
+}
+
+-(NSUInteger)indexForKey:(NSString *)key
+{
+    NSUInteger i = 0;
+    for(NSString *k in self.formulas)
+    {
+        if([k isEqualToString:key]) return i;
+        i++;
+    }
+    return NSNotFound;
+}
+
+
+- (NSInteger) tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
+{
+    if(section == ADD_SECTION)
+        return 1;
+        
+    return self.formulas.count;
+}
+
+// Override to support conditional editing of the table view.
+- (BOOL)tableView:(UITableView *)tableView canEditRowAtIndexPath:(NSIndexPath *)indexPath
+{
+    if(indexPath.section == ADD_SECTION) return false;
+    // Return NO if you do not want the specified item to be editable.
+    return true;
+}
+
+
+-(id)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
+{
+    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"EffectCell"];
+    if (!cell)
+    {
+        cell = [UITableViewCell.alloc initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"EffectCell"];
+        cell.textLabel.adjustsFontSizeToFitWidth = true;
+        //cell.editing = true;
+        //cell.shouldIndentWhileEditing = false;
+    }
+    cell.accessoryType = UITableViewCellAccessoryNone;
+    cell.selected = false;
+
+    if(indexPath.section == ADD_SECTION)
+    {
+        cell.textLabel.text = LOCALIZE(@"CREATE_NEW_FORMULA", @"Create new formula");
+        cell.imageView.image = [UIImage systemImageNamed:@"plus.app"];
+    }
+    else if(indexPath.section == FORMULA_SECTION)
+    {
+        NSString *name = [self keyForIndex:indexPath.row];
+        BOOL selected = [name isEqualToString:self.selectedFormula];
+        cell.textLabel.text = name;
+        if(selected)
+            cell.accessoryType = UITableViewCellAccessoryCheckmark;
+        cell.imageView.image = nil;
+    }
+
+    return cell;
+}
+
+-(void)tableView:(UITableView *)tableView commitEditingStyle:(UITableViewCellEditingStyle)editingStyle forRowAtIndexPath:(NSIndexPath *)indexPath
+{
+    if (editingStyle == UITableViewCellEditingStyleDelete)
+    {
+        NSString *key = [self keyForIndex:indexPath.row];
+        [self.formulas removeObjectForKey:key];
+        [tableView deleteRowsAtIndexPaths:[NSArray arrayWithObject:indexPath] withRowAnimation:UITableViewRowAnimationAutomatic];
+        [self updateSettings];
+    }
+}
+
+-(void) tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
+{
+    if(indexPath.section == ADD_SECTION)
+    {
+        CylinderSettingsListController *ctrl = (CylinderSettingsListController*)self.parentController;
+
+        NSDictionary *effects = [ctrl.settings objectForKey:PrefsEffectKey];
+
+        if(effects.count == 0)
+        {
+            UIAlertController *alert = [UIAlertController alertControllerWithTitle:LOCALIZE(@"NO_EFFECTS_ENABLED_TITLE", @"You have no effects enabled!") message:LOCALIZE(@"NO_EFFECTS_ENABLED_DESC", @"Go back to the effects list, enable some effects, then come back here and create a new formula.") preferredStyle:UIAlertControllerStyleAlert];
+            UIAlertAction* cancelButton = [UIAlertAction actionWithTitle:LOCALIZE(@"NO_EFFECTS_ENABLED_OK", @"Aight cool") style:UIAlertActionStyleCancel handler:^(UIAlertAction * action) {}];
+            [alert addAction:cancelButton];
+            [self presentViewController:alert animated:YES completion:nil];
+        }
+        else
+        {
+            [self showAlertWithText:LOCALIZE(@"CREATE_FORMULA_INFO", @"The new formula will have whatever effects you have enabled right now.")];
+        }
+    }
+    else if(indexPath.section == FORMULA_SECTION)
+    {
+        if(self.selectedFormula)
+        {
+            int index = [self indexForKey:self.selectedFormula];
+            UITableViewCell *cell = [_tableView cellForRowAtIndexPath:[NSIndexPath indexPathForRow:index inSection:FORMULA_SECTION]];
+            cell.accessoryType = UITableViewCellAccessoryNone;
+        }
+
+        self.selectedFormula = [self keyForIndex:indexPath.row];
+        UITableViewCell *cell = [_tableView cellForRowAtIndexPath:indexPath];
+        cell.accessoryType = UITableViewCellAccessoryCheckmark;
+
+        [self updateSettings];
+    }
+    [tableView deselectRowAtIndexPath:indexPath animated:true];
+}
+
+-(void)updateSettings
+{
+    // make the title changes
+    CylinderSettingsListController *ctrl = (CylinderSettingsListController*)self.parentController;
+    ctrl.formulas = self.formulas;
+    ctrl.selectedFormula = self.selectedFormula;
+    [ctrl writeSettings];
+}
+
+@end
  ```

  </details>
- 🟢 `Cylinder/settings/CLTwitterCell.h` (added) — +10 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,10 @@
+#import <UIKit/UIKit.h>
+#import <Preferences/PSSpecifier.h>
+#import <Preferences/PSTableCell.h>
+
+@interface CLTwitterCell : PSTableCell{
+    NSString *_user;
+}
+@property (nonatomic, retain, readonly) UIView *avatarView;
+@property (nonatomic, retain, readonly) UIImageView *avatarImageView;
+@end
  ```

  </details>
- 🟢 `Cylinder/settings/CLTwitterCell.m` (added) — +134 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,134 @@
+#import "CLTwitterCell.h"
+
+@implementation CLTwitterCell
+
+- (instancetype)initWithStyle:(UITableViewCellStyle)style reuseIdentifier:(NSString *)reuseIdentifier specifier:(PSSpecifier *)specifier
+{
+    self = [super initWithStyle:UITableViewCellStyleSubtitle reuseIdentifier:reuseIdentifier specifier:specifier];
+
+    if (self) {
+
+        self.selectionStyle = UITableViewCellSelectionStyleBlue;
+        self.accessoryView = [[UIImageView alloc] initWithFrame:CGRectMake( 0, 0, 38, 38)];
+
+        self.detailTextLabel.numberOfLines = 1;
+        self.detailTextLabel.textColor = [UIColor grayColor];
+
+        self.textLabel.textColor = [UIColor blackColor];
+        
+        if (@available(iOS 13, *)) {
+            self.tintColor = [UIColor labelColor];
+        }
+        else {
+            self.tintColor = [UIColor blackColor];
+        }
+
+        CGFloat const size = 29.f;
+
+        UIGraphicsBeginImageContextWithOptions(CGSizeMake(size, size), NO, [UIScreen mainScreen].scale);
+        specifier.properties[@"iconImage"] = UIGraphicsGetImageFromCurrentImageContext();
+        UIGraphicsEndImageContext();
+
+        _avatarView = [[UIView alloc] initWithFrame:self.imageView.bounds];
+        _avatarView.autoresizingMask = UIViewAutoresizingFlexibleWidth | UIViewAutoresizingFlexibleHeight;
+        _avatarView.backgroundColor = [UIColor colorWithWhite:0.9f alpha:1];
+        _avatarView.userInteractionEnabled = NO;
+        _avatarView.clipsToBounds = YES;
+        _avatarView.layer.cornerRadius = size / 2;
+        _avatarView.layer.borderWidth = 2;
+
+        if (@available(iOS 13, *)) {
+            _avatarView.layer.borderColor = [[UIColor tertiaryLabelColor] CGColor];
+        }
+        else {
+            _avatarView.layer.borderColor = [[UIColor colorWithWhite:1 alpha:0.3] CGColor];
+        }
+        
+        [self.imageView addSubview:_avatarView];
+
+        _avatarImageView = [[UIImageView alloc] initWithFrame:_avatarView.bounds];
+        _avatarImageView.autoresizingMask = UIViewAutoresizingFlexibleWidth | UIViewAutoresizingFlexibleHeight;
+        _avatarImageView.alpha = 0;
+        _avatarImageView.layer.minificationFilter = kCAFilterTrilinear;
+        [_avatarView addSubview:_avatarImageView];
+
+        _user = [specifier.properties[@"accountName"] copy];
+        NSAssert(_user, @"User name not provided");
+
+        specifier.properties[@"url"] = [self.class _urlForUsername:_user];
+
+        self.detailTextLabel.text = _user;
+
+        if (!_user) {
+            return self;
+        }
+
+        // self.avatarImage = [UIImage imageNamed:[NSString stringWithFormat:@"/Library/PreferenceBundles/little11prefs.bundle/%@.png", _user]];
+
+        // This has a delay as image needs to be downloaded
+        dispatch_async(dispatch_get_global_queue(0,0), ^{
+            
+            // NSString *size = [UIScreen mainScreen].scale > 2 ? @"original" : @"bigger";
+            NSError __block *err = NULL;
+            NSData __block *data;
+            BOOL __block reqProcessed = false;
+            NSURLRequest *request = [NSURLRequest requestWithURL:[NSURL URLWithString:[NSString stringWithFormat:@"https://pbs.twimg.com/profile_images/1161080936836018176/4GUKuGlb_200x200.jpg"]]];
+            
+            [[[NSURLSession sharedSession] dataTaskWithRequest:request completionHandler:^(NSData  *_data, NSURLResponse *_response, NSError *_error) {
+                err = _error;
+                data = _data;
+                reqProcessed = true;
+            }] resume];
+
+            while (!reqProcessed) {
+                [NSThread sleepForTimeInterval:0];
+            }
+
+            if (err)
+                return;
+                
+            dispatch_async(dispatch_get_main_queue(), ^{
+                    self.avatarImage = [UIImage imageWithData: data];
+            });
+        });
+    }
+
+    return self;
+}
+
+#pragma mark - Avatar
+
+- (void)setAvatarImage:(UIImage *)avatarImage
+{
+    _avatarImageView.image = avatarImage;
+
+    if (_avatarImageView.alpha == 0)
+    {
+        [UIView animateWithDuration:0.15
+            animations:^{
+                _avatarImageView.alpha = 1;
+            }
+        ];
+    }
+}
+
++ (NSURL *)_urlForUsername:(NSString *)user {
+
+    if ([[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:@"tweetbot://"]]) {
+        return [NSURL URLWithString: [@"tweetbot:///user_profile/" stringByAppendingString:user]];
+    } else if ([[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:@"twitterrific://"]]) {
+        return [NSURL URLWithString: [@"twitterrific:///profile?screen_name=" stringByAppendingString:user]];
+    } else if ([[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:@"tweetings://"]]) {
+        return [NSURL URLWithString: [@"tweetings:///user?screen_name=" stringByAppendingString:user]];
+    } else if ([[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:@"twitter://"]]) {
+        return [NSURL URLWithString: [@"twitter://user?screen_name=" stringByAppendingString:user]];
+    } else {
+        return [NSURL URLWithString: [@"https://mobile.twitter.com/" stringByAppendingString:user]];
+    }
+}
+
+- (void)setSelected:(BOOL)arg1 animated:(BOOL)arg2
+{
+    if (arg1) [[UIApplication sharedApplication] openURL:[self.class _urlForUsername:_user] options:@{} completionHandler:nil];
+}
+@end
\ No newline at end of file
  ```

  </details>
- 🟢 `Cylinder/settings/CylinderSettings.h` (added) — +11 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,11 @@
+#import <Preferences/PSListController.h>
+#import <Preferences/PSSpecifier.h>
+#import "CLEffect.h"
+
+@interface CylinderSettingsListController: PSListController
+@property (nonatomic, retain, readonly) NSUserDefaults *settings;
+- (void)setSelectedEffects:(NSArray *)effects;
+-(void)setSelectedFormula:(NSString *)formula;
+-(void)setFormulas:(NSDictionary *)formulas;
+- (void)writeSettings;
+@end
  ```

  </details>
- 🟢 `Cylinder/settings/CylinderSettings.m` (added) — +130 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,130 @@
+#import "CylinderSettings.h"
+#import "../Defines.h"
+#import "CLEffect.h"
+
+@interface CylinderSettingsListController()
+{
+    NSString *_defaultFooterText;
+}
+@property (nonatomic, retain, readwrite) NSUserDefaults *settings;
+@end
+
+@implementation CylinderSettingsListController
+@synthesize settings = _settings;
+
+- (instancetype)init { 
+    self = [super init];
+
+    if (self) {
+        self.settings = [[NSUserDefaults alloc] initWithSuiteName:@"com.ryannair05.cylinderremade"];
+        [self.settings registerDefaults:@{ PrefsEffectKey: DEFAULT_EFFECTS }];
+        _defaultFooterText = [[NSDictionary dictionaryWithContentsOfFile:@"/Library/PreferenceBundles/CylinderSettings.bundle/en.lproj/CylinderSettings.strings"] objectForKey:@"FOOTER_TEXT"];
+    }
+    return self;
+}
+
+- (NSArray *)specifiers {
+	if (_specifiers == nil) {
+		_specifiers = [self loadSpecifiersFromPlistName:@"CylinderSettings" target:self];
+	}
+	return _specifiers;
+}
+
+-(void)visitBarrel:(id)sender
+{
+    [[UIApplication sharedApplication] openURL:[NSURL URLWithString:@"cydia://package/com.aaronash.barrel"] options:@{} completionHandler:nil];
+}
+
+- (void)visitTwitter:(id)sender {
+    NSString* user = @"rweichler";
+    NSURL* url = [NSURL URLWithString: [@"https://twitter.com/" stringByAppendingString:user]];
+
+    if ([[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:@"tweetbot://"]]) {
+        url = [NSURL URLWithString: [@"tweetbot:///user_profile/" stringByAppendingString:user]];
+    } else if ([[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:@"twitterrific://"]]) {
+        url = [NSURL URLWithString: [@"twitterrific:///profile?screen_name=" stringByAppendingString:user]];
+    } else if ([[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:@"tweetings://"]]) {
+        url = [NSURL URLWithString: [@"tweetings:///user?screen_name=" stringByAppendingString:user]];
+    } else if ([[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:@"twitter://"]]) {
+        url = [NSURL URLWithString: [@"twitter://user?screen_name=" stringByAppendingString:user]];
+    }
+
+    [[UIApplication sharedApplication] openURL:url options:@{} completionHandler:nil];
+}
+
+- (void)visitGithub:(id)sender {
+	[[UIApplication sharedApplication] openURL:[NSURL URLWithString:@"https://github.com/ryannair05/Cylinder-Remade"] options:@{} completionHandler:nil];
+}
+
+- (void)visitReddit:(id)sender {
+    if ([[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:@"apollo://"]]) {
+        [[UIApplication sharedApplication] openURL:[NSURL URLWithString:@"apollo://reddit.com/r/cylinder"] options:@{} completionHandler:nil];
+    }
+    else {
+        [[UIApplication sharedApplication] openURL:[NSURL URLWithString:@"http://reddit.com/r/cylinder"] options:@{} completionHandler:nil];
+    }
+}
+
+- (void)writeSettings {
+
+    CFNotificationCenterPostNotification(CFNotificationCenterGetDarwinNotifyCenter(), (CFStringRef) kCylinderSettingsChanged, NULL, NULL, YES);
+}
+
+-(void)setSelectedEffects:(NSArray *)effects
+{
+    NSMutableString *text = [NSMutableString string];
+    NSMutableArray *toWrite = [NSMutableArray arrayWithCapacity:effects.count];
+    for(CLEffect *effect in effects)
+    {
+        if(!effect.name || !effect.selectorName) continue;
+
+        NSDictionary *dict = [NSDictionary dictionaryWithObjectsAndKeys:effect.name, PrefsEffectKey, effect.selectorName, PrefsEffectDirKey, nil];
+        [toWrite addObject:dict];
+
+        [text appendString:effect.name];
+        if(effect != effects.lastObject)
+        {
+            [text appendString:@", "];
+        }
+    }
+
+    UITableViewCell *cell = [self.table cellForRowAtIndexPath:[NSIndexPath indexPathForRow:1 inSection:0]];
+    cell.detailTextLabel.text = text;
+
+    [_settings setObject:toWrite forKey:PrefsEffectKey];
+    self.selectedFormula = nil;
+    [self writeSettings];
+}
+
+-(void)setFormulas:(NSDictionary *)formulas
+{
+    [_settings setObject:formulas forKey:PrefsFormulaKey];
+}
+
+-(void)setSelectedFormula:(NSString *)formula
+{
+    if(!formula)
+    {
+        [_settings removeObjectForKey:PrefsSelectedFormulaKey];
+        return;
+    }
+
+    [_settings setObject:formula forKey:PrefsSelectedFormulaKey];
+
+    NSDictionary *formulas = [_settings objectForKey:PrefsFormulaKey];
+    NSArray *effects = [formulas objectForKey:formula];
+
+    if(effects)
+        [_settings setObject:effects forKey:PrefsEffectKey];
+
+}
+
+-(NSString *)tableView:(UITableView *)tableView titleForFooterInSection:(NSInteger)section
+{
+    if(section == 1)
+        return LOCALIZE(@"FOOTER_TEXT", _defaultFooterText);
+    else
+        return [super tableView:tableView titleForFooterInSection:section];
+}
+
+@end
  ```

  </details>
- 🟢 `Cylinder/settings/Makefile` (added) — +14 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,14 @@
+include $(THEOS)/makefiles/common.mk
+
+export TARGET := iphone:clang:16.5:14.0
+
+BUNDLE_NAME = CylinderSettings
+CylinderSettings_FILES = CLAlignedTableViewCell.m CLDonateController.m CLEffect.m CLEffectsController.m CLFormulasController.m CLTwitterCell.m CylinderSettings.m
+CylinderSettings_INSTALL_PATH = /Library/PreferenceBundles
+CylinderSettings_PRIVATE_FRAMEWORKS = Preferences
+
+include $(THEOS_MAKE_PATH)/bundle.mk
+
+internal-stage::
+	$(ECHO_NOTHING)mkdir -p $(THEOS_STAGING_DIR)/Library/PreferenceLoader/Preferences$(ECHO_END)
+	$(ECHO_NOTHING)cp entry.plist $(THEOS_STAGING_DIR)/Library/PreferenceLoader/Preferences/Cylinder.plist$(ECHO_END)
\ No newline at end of file
  ```

  </details>
- 🟢 `Cylinder/settings/Resources/CylinderSettings.plist` (added) — +137 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,137 @@
+<?xml version="1.0" encoding="UTF-8"?>
+<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
+<plist version="1.0">
+<dict>
+	<key>items</key>
+	<array>
+		<dict>
+			<key>cell</key>
+			<string>PSGroupCell</string>
+			<key>footerText</key>
+            <string>TRANSLATION</string>
+			<key>isStaticText</key>
+			<true/>
+		</dict>
+		<dict>
+			<key>cell</key>
+			<string>PSSwitchCell</string>
+			<key>default</key>
+			<true/>
+			<key>defaults</key>
+			<string>com.ryannair05.cylinderremade</string>
+			<key>key</key>
+			<string>enabled</string>
+			<key>label</key>
+			<string>ENABLED</string>
+			<key>PostNotification</key>
+			<string>com.ryannair05.cylinderremade/settingsChanged</string>
+		</dict>
+		<dict>
+			<key>cell</key>
+			<string>PSLinkListCell</string>
+			<key>default</key>
+			<string></string>
+			<key>defaults</key>
+			<string>com.ryannair05.cylinderremade</string>
+			<key>detail</key>
+			<string>CLEffectsController</string>
+			<key>id</key>
+			<string>effect</string>
+			<key>key</key>
+			<string>effect</string>
+			<key>label</key>
+			<string>EFFECTS</string>
+			<key>name</key>
+			<string>Effect</string>
+		</dict>
+		<dict>
+			<key>cell</key>
+			<string>PSLinkListCell</string>
+			<key>default</key>
+			<string></string>
+			<key>defaults</key>
+			<string>com.ryannair05.cylinderremade</string>
+			<key>detail</key>
+			<string>CLFormulasController</string>
+			<key>id</key>
+			<string>formula</string>
+			<key>key</key>
+			<string>formula</string>
+			<key>label</key>
+			<string>FORMULAS</string>
+			<key>name</key>
+			<string>Formula</string>
+		</dict>
+		<dict>
+			<key>cell</key>
+			<string>PSSwitchCell</string>
+			<key>default</key>
+			<false/>
+			<key>defaults</key>
+			<string>com.ryannair05.cylinderremade</string>
+			<key>key</key>
+			<string>randomized</string>
+			<key>label</key>
+			<string>RANDOMIZE</string>
+			<key>PostNotification</key>
+			<string>com.ryannair05.cylinderremade/settingsChanged</string>
+		</dict>
+		<dict>
+			<key>cell</key>
+			<string>PSGroupCell</string>
+			<key>footerText</key>
+            <string>FOOTER_TEXT</string>
+			<key>isStaticText</key>
+			<true/>
+		</dict>
+		<dict>
+			<key>cell</key>
+			<string>PSLinkListCell</string>
+			<key>default</key>
+			<string></string>
+			<key>defaults</key>
+			<string>com.ryannair05.cylinderremade</string>
+			<key>detail</key>
+			<string>CLDonateController</string>
+			<key>id</key>
+			<string>donate</string>
+			<key>key</key>
+			<string>donate</string>
+			<key>label</key>
+			<string>DONATE</string>
+			<key>name</key>
+			<string>Donate</string>
+		</dict>
+		<dict>
+			<key>cellClass</key>
+			<string>CLTwitterCell</string>
+			<key>label</key>
+			<string>Ryan Nair</string>
+			<key>accountName</key>
+			<string>ryannair05</string>
+		</dict>
+		<dict>
+			<key>action</key>
+			<string>visitGithub:</string>
+			<key>cell</key>
+			<string>PSButtonCell</string>
+			<key>label</key>
+			<string>SOURCE_CODE</string>
+            <key>icon</key>
+            <string>github.png</string>
+		</dict>
+		<dict>
+			<key>action</key>
+			<string>visitBarrel:</string>
+			<key>cell</key>
+			<string>PSButtonCell</string>
+			<key>label</key>
+            <string>BARREL</string>
+            <key>icon</key>
+            <string>barrel.png</string>
+		</dict>
+	</array>
+	<key>title</key>
+	<string>Cylinder</string>
+</dict>
+</plist>
  ```

  </details>
- 🟢 `Cylinder/settings/Resources/Icon.png` (added) — +0 / -0
- 🟢 `Cylinder/settings/Resources/Icon@2x.png` (added) — +0 / -0
- 🟢 `Cylinder/settings/Resources/Icon@3x.png` (added) — +0 / -0
- 🟢 `Cylinder/settings/Resources/Info.plist` (added) — +28 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,28 @@
+<?xml version="1.0" encoding="UTF-8"?>
+<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
+<plist version="1.0">
+<dict>
+	<key>CFBundleDevelopmentRegion</key>
+	<string>English</string>
+	<key>CFBundleExecutable</key>
+	<string>CylinderSettings</string>
+	<key>CFBundleIdentifier</key>
+	<string>com.ryannar05.cylindersettings</string>
+	<key>CFBundleInfoDictionaryVersion</key>
+	<string>6.0</string>
+	<key>CFBundlePackageType</key>
+	<string>BNDL</string>
+	<key>CFBundleShortVersionString</key>
+	<string>1.0.0</string>
+	<key>CFBundleSignature</key>
+	<string>????</string>
+	<key>CFBundleVersion</key>
+	<string>1.0</string>
+	<key>DTPlatformName</key>
+	<string>iphoneos</string>
+	<key>MinimumOSVersion</key>
+	<string>3.0</string>
+	<key>NSPrincipalClass</key>
+	<string>CylinderSettingsListController</string>
+</dict>
+</plist>
  ```

  </details>
- 🟢 `Cylinder/settings/Resources/ar.lproj/CylinderSettings.strings` (added) — +34 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,34 @@
+{
+    ENABLED = "تمكين";
+    EFFECTS = "مؤثرات";
+    FORMULAS = "الصيغ";
+    RANDOMIZE = "عشوائي";
+    TRANSLATION = "ترجمة حسين إبراهيم عباس";
+
+    DONATE = "ساهم بنجاحنا";
+    SOURCE_CODE = "شفرة المصدر";
+    R_CYLINDER = "المزيد من المؤثرات على  /r/cylinder";
+    BARREL = "Barrel (الأداة الأصلية)";
+
+    //Effects subscreen
+
+    RESET_EFFECTS = "حذف";
+
+    //Formulas subscreen
+
+    CREATE_NEW_FORMULA = "انتاج صيغة جديدة";
+    CREATE_FORMULA_INFO = "الصيغة الجديدة ستحتوي على كل المؤثرات التي ستقوم بتفعيلها الآن.";
+    FORMULA_NAME = "اسم الصيغة";
+    CREATE_FORMULA = "أنتج الصيغة";
+    CANCEL = "إلغاء";
+    FORMULA_ALREADY_EXISTS = "هناك صيغة حالية موجودة بنفس الاسم.";
+    OVERWRITE_IT = "استبدال";
+    NO_EFFECTS_ENABLED_TITLE = "لا يوجد لديك أي مؤثرات ممكنة!";
+    NO_EFFECTS_ENABLED_DESC = "يرجى العودة لقائمة المؤثرات وتفعيل البعض منها ومن ثم العودة هنا لعمل صيغة جديدة.";
+    NO_EFFECTS_ENABLED_OK = "حسناً";
+
+    //Donate subscreen
+
+    THANK_YOU = "شكراً لك";
+    PAYPAL = "اضغط هنا ليتم تشغيل متصفح سفاري ليتم التبرع عبر PayPal";
+}
  ```

  </details>
- 🟢 `Cylinder/settings/Resources/barrel@2x.png` (added) — +0 / -0
- 🟢 `Cylinder/settings/Resources/cs.lproj/CylinderSettings.strings` (added) — +38 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,38 @@
+{
+    //Main part
+
+    ENABLED = "Zapnuto";
+    EFFECTS = "Efekty";
+    FORMULAS = "Kombinace";
+    RANDOMIZE = "Náhodně";
+
+    //Credit yourself here (you don't have to if you don't want to)
+    TRANSLATION = "Přeložil Sh4rP EYE (@MrSh4rPEYE)";
+
+    DONATE = "Přispět";
+    SOURCE_CODE = "Zdrojový kód";
+    R_CYLINDER = "Více efektů na /r/cylinder";
+    BARREL = "Barrel (původní tweak)";
+
+    //Effects subscreen
+
+    RESET_EFFECTS = "Vyčistit";
+
+    //Formulas subscreen
+
+    CREATE_NEW_FORMULA = "Nová kombinace";
+    CREATE_FORMULA_INFO = "Nová kombinace bude obsahovat všechny efekty, které máte zrovna zapnuté.";
+    FORMULA_NAME = "Jméno kombinace";
+    CREATE_FORMULA = "Vytvořit";
+    CANCEL = "Zrušit";
+    FORMULA_ALREADY_EXISTS = "Kombinace s tímto jménem už existuje.";
+    OVERWRITE_IT = "Přepsat";
+    NO_EFFECTS_ENABLED_TITLE = "Nemáte zapnuté žádné efekty!";
+    NO_EFFECTS_ENABLED_DESC = "Vraťte se zpět do seznamu efektů, nějaké z nich zapněte a poté přijďte zpátky sem a vytvořte novou kombinaci.";
+    NO_EFFECTS_ENABLED_OK = "Ahá, jasný";
+
+    //Donate subscreen
+
+    THANK_YOU = "Děkuji Vám";
+    PAYPAL = "Klikněte zde pro přechod do Safari, kde budete moci přispět přes PayPal";
+}
  ```

  </details>
- 🟢 `Cylinder/settings/Resources/da.lproj/CylinderSettings.strings` (added) — +37 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,37 @@
+{
+    //Main part
+
+    ENABLED = "Aktiveret";
+    EFFECTS = "Effekter";
+    FORMULAS = "Formularer";
+    RANDOMIZE = "Bland";
+
+    TRANSLATION = "Denne tweak er oversat af Jonas Lindenskov Nielsen (@jlndk)";
+
+    DONATE = "Donér";
+    SOURCE_CODE = "Bundkode";
+    R_CYLINDER = "Flere effekter på /r/cylinder";
+    BARREL = "Barrel (original tweak)";
+
+    //Effects subscreen
+
+    RESET_EFFECTS = "Ryd";
+
+    //Formulas subscreen
+
+    CREATE_NEW_FORMULA = "Opret ny formular";
+    CREATE_FORMULA_INFO = "Formularen vil indeholde de effekter du har aktivet nu.";
+    FORMULA_NAME = "Formular Navn";
+    CREATE_FORMULA = "Opret Formular";
+    CANCEL = "Annuller";
+    FORMULA_ALREADY_EXISTS = "Der eksistere allerede en formular med dette navn.";
+    OVERWRITE_IT = "Overskriv";
+    NO_EFFECTS_ENABLED_TITLE = "Du har ingen effekter aktiveret!";
+    NO_EFFECTS_ENABLED_DESC = "Gå tilbage til effektlisten, aktiver nogle effekter, og kom tilbage hertil for at oprette en ny formular.";
+    NO_EFFECTS_ENABLED_OK = "Ok, cool";
+
+    //Donate subscreen
+
+    THANK_YOU = "Mange tak";
+    PAYPAL = "Tryk her for at åbne PayPal i Safari";
+}
  ```

  </details>
- 🟢 `Cylinder/settings/Resources/de.lproj/CylinderSettings.strings` (added) — +25 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,25 @@
+{
+    "BARREL" = "Barrel (Original Tweak)";
+    "CANCEL" = "Abbrechen";
+    "CREATE_FORMULA" = "Formel erzeugen";
+    "CREATE_FORMULA_INFO" = "Die neue Formel wird jeden Effekt beinhalten, den du jetzt aktivierst.";
+    "CREATE_NEW_FORMULA" = "Neue Formel erzeugen";
+    "DONATE" = "Spenden";
+    "EFFECTS" = "Effekte";
+    "ENABLED" = "Aktiviert";
+    "FOOTER_TEXT" = "WIE ERSTELLT MAN EFFEKTE:\n\nÖffne iFile (oder Filza) und gehe zu /Library/Cylinder. Dort siehst du eine Menge Ordner, die mit Usernames benannt sind. In diesen Ordnern befinden sich ein paar .lua-Dateien. Öffne irgendeine mit dem Text-Editor und du wirst den Code sehen. Für den Anfang empfehle ich die Backwards.lua Datei im gertab-Ordner anzuschauen, um eine Idee davon zu bekommen wie es funktioniert. Wenn du in der Datei den 2*offset zu 3*offset änderst, (Vergiss nicht den Effekt anschließend neu zu laden!) wirst du eine Art von \"versetztem Backwards\" - Effekt bemerken.\n\nFolge dieser Vorgehensweise um eigene Effekte zu erstellen. Wenn du Probleme bekommst, schau dir die EXAMPLE.lua Datei oder ein Tutorial auf YouTube an.";
+    "FORMULA_ALREADY_EXISTS" = "Eine Formel mit diesem Namen existiert bereits.";
+    "FORMULA_NAME" = "Name der Formel";
+    "FORMULAS" = "Formeln";
+    "NO_EFFECTS_ENABLED_DESC" = "Gehe zurück zur Effekt-Liste, aktiviere ein paar Effekte und versuche dein Glück erneut.";
+    "NO_EFFECTS_ENABLED_OK" = "Ok, alles klar...";
+    "NO_EFFECTS_ENABLED_TITLE" = "Du hast keine Effekte aktiviert!";
+    "OVERWRITE_IT" = "Überschreiben";
+    "PAYPAL" = "Hier tippen um Safari zu öffnen und via PayPal zu spenden.";
+    "R_CYLINDER" = "Mehr Effekte auf /r/cylinder";
+    "RANDOMIZE" = "Zufällig";
+    "RESET_EFFECTS" = "Zurücksetzen";
+    "SOURCE_CODE" = "Quell-Code";
+    "THANK_YOU" = "Danke";
+    "TRANSLATION" = "Übersetzungen gesucht! Wenn du dieses Tweak übersetzen willst, schick mir eine E-Mail oder einen Tweet.";
+}
  ```

  </details>
- 🟢 `Cylinder/settings/Resources/en.lproj/CylinderSettings.strings` (added) — +48 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,48 @@
+{
+    //Main part
+
+    ENABLED = "Enabled";
+    EFFECTS = "Effects";
+    FORMULAS = "Formulas";
+    RANDOMIZE = "Randomize";
+
+    //Don't translate this bit, credit yourself here (if you want)
+    TRANSLATION = "Translations wanted! If you'd like to translate this tweak shoot me a tweet or an email";
+
+    DONATE = "Donate";
+    SOURCE_CODE = "Source code";
+    R_CYLINDER = "More effects on /r/cylinder";
+    BARREL = "Barrel (original tweak)";
+
+    //Effects subscreen
+
+    RESET_EFFECTS = "Clear";
+
+    //Formulas subscreen
+
+    CREATE_NEW_FORMULA = "Create new formula";
+    CREATE_FORMULA_INFO = "The new formula will have whatever effects you have enabled right now.";
+    FORMULA_NAME = "Formula name";
+    CREATE_FORMULA = "Create Formula";
+    CANCEL = "Cancel";
+    FORMULA_ALREADY_EXISTS = "A formula with that name already exists.";
+    OVERWRITE_IT = "Overwrite it";
+    NO_EFFECTS_ENABLED_TITLE = "You have no effects enabled!";
+    NO_EFFECTS_ENABLED_DESC = "Go back to the effects list, enable some effects, then come back here and create a new formula.";
+    NO_EFFECTS_ENABLED_OK = "Aight cool";
+
+    //Donate subscreen
+
+    THANK_YOU = "Thank you";
+    PAYPAL = "Tap here to go to Safari and donate via PayPal";
+
+    //Footer text
+
+    //You do not have to translate this part if you don't want to. I realize it's a lot of work.
+
+    FOOTER_TEXT = "HOW TO CODE YOUR OWN EFFECTS:
+
+Click Source Code and you will be taken to this project's Github page. Go to /tweak/CylinderAnimator.swift where you will see the code for all the effects. You can add your own static method to the CylinderAnimator class and the go to -addEffectsFromDirectory: in settings/CLEffectsController.m and add your own CLEffect object to the effects array filling out its name, selector, and your name
+
+Follow the pattern you see in order to add your own effects. If you get stuck, take a look at the examples or search \"Swift tutorial\" on YouTube.";
+}
  ```

  </details>
- 🟢 `Cylinder/settings/Resources/error.png` (added) — +0 / -0
- 🟢 `Cylinder/settings/Resources/error@2x.png` (added) — +0 / -0
- 🟢 `Cylinder/settings/Resources/es_MX.lproj/CylinderSettings.strings` (added) — +38 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,38 @@
+{
+    //Main part
+
+    ENABLED = "Habilitado";
+    EFFECTS = "Efectos";
+    FORMULAS = "Fórmulas";
+    RANDOMIZE = "Aleatorio";
+
+    //Credit yourself here (you don't have to if you don't want to)
+    TRANSLATION = "Traducido por Franco Rudelir (@EpicRudelir en Twitter)";
+
+    DONATE = "Donar";
+    SOURCE_CODE = "Código fuente";
+    R_CYLINDER = "Más efectos en /r/cylinder";
+    BARREL = "Barrel (tweak original)";
+
+    //Effects subscreen
+
+    RESET_EFFECTS = "Restaurar efectos";
+
+    //Formulas subscreen
+
+    CREATE_NEW_FORMULA = "Crear nueva fórmula";
+    CREATE_FORMULA_INFO = "La nueva fórmula será guardada con los efectos que están habilitados en este momento.";
+    FORMULA_NAME = "Nombre de la fórmula";
+    CREATE_FORMULA = "Crear fórmula";
+    CANCEL = "Cancelar";
+    FORMULA_ALREADY_EXISTS = "Ya existe una fórmula con ese nombre.";
+    OVERWRITE_IT = "Sobreescribirla";
+    NO_EFFECTS_ENABLED_TITLE = "No tienes ningún efecto activado!";
+    NO_EFFECTS_ENABLED_DESC = "Ve a la lista de efectos, activa algunos, luego vuelva y crea una nueva fórmula.";
+    NO_EFFECTS_ENABLED_OK = "Listo";
+
+    //Donate subscreen
+
+    THANK_YOU = "Gracias";
+    PAYPAL = "Presiona aquí para ir a Safari y donar via PayPal";
+}
  ```

  </details>
- 🟢 `Cylinder/settings/Resources/fr.lproj/CylinderSettings.strings` (added) — +37 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,37 @@
+{
+    //Main part
+
+    ENABLED = "Activé";
+    EFFECTS = "Effets";
+    FORMULAS = "Formules";
+    RANDOMIZE = "Aléatoire";
+
+    TRANSLATION = "J'ai besoin de traducteurs, si voulez traduire le tweak, envoyez-moi un tweet ou un email";
+
+    DONATE = "Dons";
+    SOURCE_CODE = "Code source";
+    R_CYLINDER = "Plus d'effets sur /r/cylinder";
+    BARREL = "Barrel (tweak original)";
+
+    //Effects subscreen
+
+    RESET_EFFECTS = "Effacer";
+
+    //Formulas subscreen
+
+    CREATE_NEW_FORMULA = "Créer une nouvelle formule";
+    CREATE_FORMULA_INFO = "La nouvelle formule aura les effets activés actuellement.";
+    FORMULA_NAME = "Nom de la formule";
+    CREATE_FORMULA = "Créer une Formule";
+    CANCEL = "Annuler";
+    FORMULA_ALREADY_EXISTS = "Ce nom de formule existe déjà.";
+    OVERWRITE_IT = "Remplacer";
+    NO_EFFECTS_ENABLED_TITLE = "Vous n'avez activé aucun effet!";
+    NO_EFFECTS_ENABLED_DESC = "Retournez à la liste des effets, activez en, revenez ensuite créer une nouvelle formule.";
+    NO_EFFECTS_ENABLED_OK = "Pas cool";
+
+    //Donate subscreen
+
+    THANK_YOU = "Merci";
+    PAYPAL = "Tapez ici pour aller sur Safari et faire un don via PayPal";
+}
\ No newline at end of file
  ```

  </details>
- 🟢 `Cylinder/settings/Resources/github@2x.png` (added) — +0 / -0
- 🟢 `Cylinder/settings/Resources/he.lproj/CylinderSettings.strings` (added) — +62 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,62 @@
+<?xml version="1.0" encoding="UTF-8"?>
+<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
+<plist version="1.0">
+<dict>
+	<key>BARREL</key>
+	<string>Barrel (הטוויק המקורי)</string>
+	<key>CANCEL</key>
+	<string>בטל</string>
+	<key>CREATE_FORMULA</key>
+	<string>צור נוסחה</string>
+	<key>CREATE_FORMULA_INFO</key>
+	<string>לנוסחה החדשה יהיו האפקטים שמופעלים כרגע.</string>
+	<key>CREATE_NEW_FORMULA</key>
+	<string>צור נוסחה חדשה</string>
+	<key>DONATE</key>
+	<string>תרום</string>
+	<key>EFFECTS</key>
+	<string>אפקטים</string>
+	<key>ENABLED</key>
+	<string>הפעל</string>
+	<key>FOOTER_TEXT</key>
+	<string>איך לקודד אפקטים משלך:
+
+פתח את iFile (או Filza) ולך ל/Library/Cylinder.
+שם תראה הרבה תיקיות עם שמות משתמשים של אנשים.
+בתיקיות אלו תראה הרבה קבצי lua.
+אם תפתח אחד מהם עם קורא טקסט תוכל לראות את הקוד.
+להתחלה אני ממליץ להסתכל על על קובץ הBackwards.lua בתיקיית הgertab כדי לקבל רעיון כללי של איך לעשות את זה.
+שם אם תערוך את ה2*offset ל3*offset (אל תשכח לטעון מחדש את האפקט לאחר מכן) תשים לב לסוג של אפקט ״Backwards hella far״ (האייקונים ילכו אחורה ויהיה מרחק רב בין דף לדף).
+
+עקוב אחרי הדפוס שאתה רואה כדי להוסיף אפקטים משלך. 
+אם תיתקע, תסתכל על EXAMPLE.lua או חפש "lua tutorial" בYouTube.</string>
+	<key>FORMULAS</key>
+	<string>נוסחאות</string>
+	<key>FORMULA_ALREADY_EXISTS</key>
+	<string>נוסחה עם שם זה קיימת כבר</string>
+	<key>FORMULA_NAME</key>
+	<string>שם הנוסחה</string>
+	<key>NO_EFFECTS_ENABLED_DESC</key>
+	<string>חזור לרשימת האפקטים, הפעל כמה אפקטים, ואז חזור הנה וצור נוסחה חדשה.</string>
+	<key>NO_EFFECTS_ENABLED_OK</key>
+	<string>אוקיי</string>
+	<key>NO_EFFECTS_ENABLED_TITLE</key>
+	<string>אין לך אפקטים מופעלים!</string>
+	<key>OVERWRITE_IT</key>
+	<string>שכתב</string>
+	<key>PAYPAL</key>
+	<string>לחץ כאן כדי ללכת לספארי ולתרום דרך PayPal</string>
+	<key>RANDOMIZE</key>
+	<string>אקראי</string>
+	<key>RESET_EFFECTS</key>
+	<string>נקה</string>
+	<key>R_CYLINDER</key>
+	<string>עוד אפקטים ב/r/cylinder</string>
+	<key>SOURCE_CODE</key>
+	<string>קוד מקור</string>
+	<key>THANK_YOU</key>
+	<string>תודה</string>
+	<key>TRANSLATION</key>
+	<string>תרגום לעברית על ידי @Aranshavit</string>
+</dict>
+</plist>
  ```

  </details>
- 🟢 `Cylinder/settings/Resources/hr.lproj/CylinderSettings.strings` (added) — +44 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,44 @@
+{
+    //Main part
+
+    ENABLED = "Uključeno";
+    EFFECTS = "Efekti";
+    FORMULAS = "Formule";
+    RANDOMIZE = "Nasumično";
+
+    //Credit yourself here (you don't have to if you don't want to)
+    TRANSLATION = "Preveo Patrik Perčinić";
+
+    DONATE = "Doniraj";
+    SOURCE_CODE = "Izvorni kod";
+    R_CYLINDER = "Još efekta na /r/cylinder";
+    BARREL = "Barrel (originalni tweak)";
+
+    //Effects subscreen
+
+    RESET_EFFECTS = "Poništi odabir";
+
+    //Formulas subscreen
+
+    CREATE_NEW_FORMULA = "Izradi novu formulu";
+    CREATE_FORMULA_INFO = "Nova formula će imati efekte koji su trenutno izabrani.";
+    FORMULA_NAME = "Ime formule";
+    CREATE_FORMULA = "Napravi formulu";
+    CANCEL = "Otkaži";
+    FORMULA_ALREADY_EXISTS = "Formula sa tim imenom već postoji";
+    OVERWRITE_IT = "Prepiši ga";
+    NO_EFFECTS_ENABLED_TITLE = "Nije izabran niti jedan efekt";
+    NO_EFFECTS_ENABLED_DESC = "Vrati se na listu efekta, izaberi neke efekte, onda se vrati ovdje i napravi novu formulu.";
+    NO_EFFECTS_ENABLED_OK = "Aight cool";
+
+    //Donate subscreen
+
+    THANK_YOU = "Hvala ti!";
+    PAYPAL = "Dodirnite ovdje da biste otišli u Safari i donirali putem PayPal-a";
+
+    FOOTER_TEXT = "KAKO KODIRATI SVOJE EFEKTE:
+
+Otvorite iFile (ili Filza) i idite na / Library / Cylinder. Tamo ćete vidjeti hrpu mapa s korisničkim imenima ljudi. U tim mapama vidjet ćete hrpu .lua datoteka. Ako otvorite bilo koji od ovih pomoću preglednika teksta, možete vidjeti kod. Za početak bih preporučio da pogledate datoteku Backwards.lua u mapi gertab kako biste dobili ideju kako se kodiraju efekti. Tamo, ako uredite 2*offset na 3*offset * (ne zaboravite ponovo učitati efekt), primijetit ćete da ste napravili novi efekt.
+
+Izmjenjujte kod u drugim datotekama kako biste dodali vlastite efekte ili napišite svoj ispočetka. Ako zapnete, pogledajte EXAMPLE.lua ili potražite \"lua tutorial \" na YouTubu.";
+}
  ```

  </details>
- 🟢 `Cylinder/settings/Resources/iloveyou.mp3` (added) — +0 / -0
- 🟢 `Cylinder/settings/Resources/it.lproj/CylinderSettings.strings` (added) — +38 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,38 @@
+{
+    //Main part
+
+    ENABLED = "Attivato";
+    EFFECTS = "Effetti";
+    FORMULAS = "Combinazioni";
+    RANDOMIZE = "Casuale";
+
+    //Credit yourself here (you don't have to if you don't want to)
+    TRANSLATION = "Traduzione italiana di: Omega33";
+
+    DONATE = "Dona qui";
+    SOURCE_CODE = "Codice sorgente";
+    R_CYLINDER = "Altri effetti in /r/cylinder";
+    BARREL = "Barrel (tweak originale)";
+
+    //Effects subscreen
+
+    RESET_EFFECTS = "Cancella";
+
+    //Formulas subscreen
+
+    CREATE_NEW_FORMULA = "Crea una nuova combinazione";
+    CREATE_FORMULA_INFO = "La nuova combinazione avrà tutti gli effetti attivati in questo momento.";
+    FORMULA_NAME = "Nome della combinazione";
+    CREATE_FORMULA = "Crea combinazione";
+    CANCEL = "Annulla";
+    FORMULA_ALREADY_EXISTS = "Lo stesso nome è stato usato per un'altra combinazione!";
+    OVERWRITE_IT = "Sovrascrivila";
+    NO_EFFECTS_ENABLED_TITLE = "Non hai nessun effetto attivato!";
+    NO_EFFECTS_ENABLED_DESC = "Torna alla lista degli effetti, attiva qualche effetto, dopo torna qui e crea una nuova combinazione.";
+    NO_EFFECTS_ENABLED_OK = "Ok!";
+
+    //Donate subscreen
+
+    THANK_YOU = "Grazie";
+    PAYPAL = "Clicca qui per donare con PayPal tramite Safari";
+}
\ No newline at end of file
  ```

  </details>
- 🟢 `Cylinder/settings/Resources/ja.lproj/Localizable.strings` (added) — +0 / -0
- 🟢 `Cylinder/settings/Resources/nl.lproj/CylinderSettings.strings` (added) — +38 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,38 @@
+{
+    //Main part
+
+    ENABLED = "aan";
+    EFFECTS = "Effecten";
+    FORMULAS = "Formules";
+    RANDOMIZE = "Willekeurig";
+
+    //Credit yourself here (you don't have to if you don't want to)
+    TRANSLATION = "Vertaald door Collin Smits";
+
+    DONATE = "Doneer";
+    SOURCE_CODE = "Project code";
+    R_CYLINDER = "Meer effecten /r/cylinder";
+    BARREL = "Barrel (originele tweak)";
+
+    //Effects subscreen
+
+    RESET_EFFECTS = "reset";
+
+    //Formulas subscreen
+
+    CREATE_NEW_FORMULA = "maak een nieuwe formule";
+    CREATE_FORMULA_INFO = "De nieuwe formule bevat alle effecten die u heeft geselecteerd.";
+    FORMULA_NAME = "Naam van de formule";
+    CREATE_FORMULA = "Maak een formule";
+    CANCEL = "Annuleer";
+    FORMULA_ALREADY_EXISTS = "Er is al een formule met deze naam.";
+    OVERWRITE_IT = "Vervangen";
+    NO_EFFECTS_ENABLED_TITLE = "Er zijn geen effecten geselecteerd!";
+    NO_EFFECTS_ENABLED_DESC = "Ga terug en selecteer een paar effecten en probeer het opnieuw.";
+    NO_EFFECTS_ENABLED_OK = "Oke cool";
+
+    //Donate subscreen
+
+    THANK_YOU = "Dank u";
+    PAYPAL = "Klik hier om safari te openen en te doneren";
+}
  ```

  </details>
- 🟢 `Cylinder/settings/Resources/pt.lproj/CylinderSettings.strings` (added) — +48 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,48 @@
+{
+    //Main part
+
+    ENABLED = "Ativado";
+    EFFECTS = "Efeitos";
+    FORMULAS = "Formulas";
+    RANDOMIZE = "De forma aleatória";
+
+    //credit yourself here (if you want)
+    TRANSLATION = "Tradução por RubenEnriques7";
+
+    DONATE = "Doar";
+    SOURCE_CODE = "Código fonte";
+    R_CYLINDER = "Mais efeitor em /r/cylinder";
+    BARREL = "Barrel (tweak original)";
+
+    //Effects subscreen
+
+    RESET_EFFECTS = "Limpar";
+
+    //Formulas subscreen
+
+    CREATE_NEW_FORMULA = "Criar nova formula";
+    CREATE_FORMULA_INFO = "Esta nova formula irá conter quaisquer efeitos que estejam ativos agora.";
+    FORMULA_NAME = "Nome da formula";
+    CREATE_FORMULA = "Criar Formula";
+    CANCEL = "Cancelar";
+    FORMULA_ALREADY_EXISTS = "Já existe uma formula com o mesmo nome.";
+    OVERWRITE_IT = "Substituir";
+    NO_EFFECTS_ENABLED_TITLE = "Não tens nenhum efeito ativo!";
+    NO_EFFECTS_ENABLED_DESC = "Volta atrás até à lista de efeitos, ativa alguns efeitos, e depois volta aqui e cria uma nova formula.";
+    NO_EFFECTS_ENABLED_OK = "Ok, Boa";
+
+    //Donate subscreen
+
+    THANK_YOU = "Obrigado";
+    PAYPAL = "Pressiona aqui para ir para o Safari e doar através do Paypal.";
+
+    //Footer text
+
+    //You do not have to translate this part if you don't want to. I realize it's a lot of work.
+
+    FOOTER_TEXT = "COMO CODIFICAR OS TEUS PROPRIOS EFEITOS:
+
+Abre o iFile (ou Filza) e vai a /Library/Cylinder. Aí verás um monte de pastas com nomes de contas. Nessas pastas vais ver um monte de ficheiros .lua. Se abrires algum desses ficheiros com o editor de texto conseguirás ver o código. Para começar recomndaria ver o ficheiro Backwards.lua na pasta gertab para ter uma ideia sobre como fazê-lo. Lá dentro, se editares o 2*offset para 3*offset (não esquecer de fazer reload aos efeitos em seguida) verás uma espécie de efeito tipo \"Backwards hella far\".
+
+Segue o padrão que vês para adicionar os teus próprios efeitos. Se ficares bloqueado, dá uma olhada no ficheiro EXAMPLE.lua ou pesquisa \"lua tutorial\" no YouTube.";
+}
  ```

  </details>
- 🟢 `Cylinder/settings/Resources/ru.lproj/CylinderSettings.strings` (added) — +37 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,37 @@
+{
+    //Main part
+    ENABLED = "включено";
+    EFFECTS = "эффекты";
+    FORMULAS = "формулы";
+    RANDOMIZE = "случайный порядок";
+
+    //Don't translate this bit, credit yourself here (if you want)
+
+    TRANSLATION = "Переведено Леонидом (мой имейл leonid0079@gmail.com )";
+    DONATE = "пожертвовать";
+    SOURCE_CODE = "Исходник";
+    R_CYLINDER = "Больше эффектов на  /r/cylinder";
+    BARREL = "Barrel (с него взята идея)";
+
+    //Effects subscreen
+
+    RESET_EFFECTS = "очистить";
+
+    //Formulas subscreen
+
+    CREATE_NEW_FORMULA = "Создать новую формулу";
+    CREATE_FORMULA_INFO = "В новой формуле будут все эффекты которые включены в данный момент.";
+    FORMULA_NAME = "Название формулы";
+    CREATE_FORMULA = "Создать формулу";
+    CANCEL = "отменить";
+    FORMULA_ALREADY_EXISTS = "Уже есть формула с таким названием.";
+    OVERWRITE_IT = "Переписать это?";
+    NO_EFFECTS_ENABLED_TITLE = "Вы не включили эффекты!";
+    NO_EFFECTS_ENABLED_DESC = "Вернись обратно в список эффектов и выбери несколько эффектов потом возвращайся сюда.";
+    NO_EFFECTS_ENABLED_OK = "Ок";
+
+    //Donate subscreen
+
+    THANK_YOU = "Спасибо";
+    PAYPAL = "Нажмите здесь, чтобы перейти к Safari и пожертвовать через PayPal";
+}
  ```

  </details>
- 🟢 `Cylinder/settings/Resources/sv.lproj/CylinderSettings.strings` (added) — +37 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,37 @@
+{
+    //Main part
+
+    ENABLED = "Aktiverad";
+    EFFECTS = "Effekter";
+    FORMULAS = "Formler";
+    RANDOMIZE = "Slumpmässigt";
+
+    TRANSLATION = "Översatt Av Sebastian (Grodda Da Lo)";
+
+    DONATE = "Donera";
+    SOURCE_CODE = "Källkod";
+    R_CYLINDER = "Mer effekter på /r/cylinder";
+    BARREL = "Barrel (Original tweak)";
+
+    //Effects subscreen
+
+    RESET_EFFECTS = "Clear";
+
+    //Formulas subscreen
+
+    CREATE_NEW_FORMULA = "Skapa ny formel";
+    CREATE_FORMULA_INFO = "Den nya formeln kommer ha vilka effekter som du har valt.";
+    FORMULA_NAME = "Formel namn";
+    CREATE_FORMULA = "Skapa Formel";
+    CANCEL = "Avbryt";
+    FORMULA_ALREADY_EXISTS = "En formel med det namnet finns redan.";
+    OVERWRITE_IT = "Skriva Över";
+    NO_EFFECTS_ENABLED_TITLE = "Du har inte igång några effekter";
+    NO_EFFECTS_ENABLED_DESC = "Gå tillbaka till effekt listan och sätt igång några, sedan kom tillbaka hit och skapa en ny formula.";
+    NO_EFFECTS_ENABLED_OK = "Det är lungt";
+
+    //Donate subscreen
+
+    THANK_YOU = "Tack så mycket";
+    PAYPAL = "Tryck här för att gå till Safari och donera via PayPal";
+}
  ```

  </details>
- 🟢 `Cylinder/settings/Resources/tr.lproj/CylinderSettings.strings` (added) — +37 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,37 @@
+{
+    //Main part
+    ENABLED = "Aktif";
+    EFFECTS = "Efektler";
+    FORMULAS = "Kişiselleştir";
+    RANDOMIZE = "Rastgele";
+
+    //Credit yourself here (you don't have to if you don't want to)
+    TRANSLATION = "Sezer Çelekoğlu tarafından Türkçeleştirildi.";
+
+    DONATE = "Bağış";
+    SOURCE_CODE = "Kaynak Kodu";
+    R_CYLINDER = "Daha fazlası için /r/cylinder";
+    BARREL = "Barrel (Orjinal Uygulama)";
+
+    //Effects subscreen
+
+    RESET_EFFECTS = "Temizle";
+
+    //Formulas subscreen
+
+    CREATE_NEW_FORMULA = "Yeni Bir Liste Yarat";
+    CREATE_FORMULA_INFO = "Yeni listede sahip olduğun bütün efektler açık olur.";
+    FORMULA_NAME = "Listenin Adı";
+    CREATE_FORMULA = "Liste Yarat";
+    CANCEL = "İptal";
+    FORMULA_ALREADY_EXISTS = "Bu isimde listen zaten bulunmakta.";
+    OVERWRITE_IT = "Üzerine Yaz";
+    NO_EFFECTS_ENABLED_TITLE = "Aktif efektin bulunmamakta!";
+    NO_EFFECTS_ENABLED_DESC = "Efekt listesine geri dön, efektleri aktif et, Sonra buradan yeni bir liste yarat.";
+    NO_EFFECTS_ENABLED_OK = "Tamamlandı";
+
+    //Donate subscreen
+
+    THANK_YOU = "Teşekkürler";
+    PAYPAL = "Buraya tıkla ve Safariiye git. Paypal üzerinden Bağış yap";
+}
  ```

  </details>
- 🟢 `Cylinder/settings/Resources/zh_CN.lproj/CylinderSettings.strings` (added) — +36 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,36 @@
+<?xml version="1.0" encoding="UTF-8"?>
+<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
+<plist version="1.0">
+<dict>
+	<key>@rweichler</key>
+	<string>推特@rweichler</string>
+	<key>BARREL</key>
+	<string>Barrel (其他调整插件)</string>
+	<key>Cylinder</key>
+	<string>特效调整（Cylinder）</string>
+	<key>DONATE</key>
+	<string>    捐      助</string>
+	<key>EFFECTS</key>
+	<string>效果</string>
+	<key>ENABLED</key>
+	<string>启用</string>
+	<key>NO_EFFECTS_ENABLED_OK</key>
+	<string>OK</string>
+	<key>OVERWRITE_IT</key>
+	<string>覆盖它</string>
+	<key>PAYPAL</key>
+	<string>点击这里进入Safari浏览器，并通过贝宝 (PayPal)捐助</string>
+	<key>RANDOMIZE</key>
+	<string>随机</string>
+	<key>RESET_EFFECTS</key>
+	<string>取消</string>
+	<key>R_CYLINDER</key>
+	<string>更多特效在/r/cylinder</string>
+	<key>SOURCE_CODE</key>
+	<string>源   代   码</string>
+	<key>THANK_YOU</key>
+	<string>感谢您捐助</string>
+	<key>TRANSLATION</key>
+	<string>如果你想把这个调整翻译请推特联系我或发电子邮件</string>
+</dict>
+</plist>
  ```

  </details>
- 🟢 `Cylinder/settings/Resources/zh_TW.lproj/CylinderSettings.strings` (added) — +36 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,36 @@
+<?xml version="1.0" encoding="UTF-8"?>
+<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
+<plist version="1.0">
+<dict>
+	<key>@rweichler</key>
+	<string>推特@rweichler</string>
+	<key>BARREL</key>
+	<string>Barrel （其他調整插件）</string>
+	<key>Cylinder</key>
+	<string>特效調整（Cylinder）</string>
+	<key>DONATE</key>
+	<string>    捐      助</string>
+	<key>EFFECTS</key>
+	<string>效果</string>
+	<key>ENABLED</key>
+	<string>啟用</string>
+	<key>NO_EFFECTS_ENABLED_OK</key>
+	<string>OK</string>
+	<key>OVERWRITE_IT</key>
+	<string>覆蓋它</string>
+	<key>PAYPAL</key>
+	<string>點擊這裡進入的Safari瀏覽器，並通過貝寶（貝寶）捐助</string>
+	<key>RANDOMIZE</key>
+	<string>隨機</string>
+	<key>RESET_EFFECTS</key>
+	<string>取消</string>
+	<key>R_CYLINDER</key>
+	<string>更多特效在/r/cylinder</string>
+	<key>SOURCE_CODE</key>
+	<string>源  代  碼</string>
+	<key>THANK_YOU</key>
+	<string>感谢您捐助</string>
+	<key>TRANSLATION</key>
+	<string>如果你想把這個調整翻譯請推特聯繫我或發電子郵件</string>
+</dict>
+</plist>
  ```

  </details>
- 🟢 `Cylinder/settings/entry.plist` (added) — +21 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,21 @@
+<?xml version="1.0" encoding="UTF-8"?>
+<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
+<plist version="1.0">
+<dict>
+	<key>entry</key>
+	<dict>
+		<key>bundle</key>
+		<string>CylinderSettings</string>
+		<key>cell</key>
+		<string>PSLinkCell</string>
+		<key>detail</key>
+		<string>CylinderSettingsListController</string>
+		<key>icon</key>
+		<string>Icon.png</string>
+		<key>isController</key>
+		<true/>
+		<key>label</key>
+		<string>Cylinder Remade</string>
+	</dict>
+</dict>
+</plist>
  ```

  </details>
- 🟢 `Cylinder/tweak/CALayer+Cylinder.h` (added) — +6 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,6 @@
+#import <UIKit/UIView.h>
+
+@interface CALayer(Cylinder)
+-(void)savePosition;
+-(void)restorePosition;
+@end
  ```

  </details>
- 🟢 `Cylinder/tweak/CALayer+Cylinder.m` (added) — +28 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,28 @@
+#import "CALayer+Cylinder.h"
+#import <objc/runtime.h>
+
+@implementation CALayer(Cylinder)
+-(void)setSavedValue:(NSValue *)value
+{
+    objc_setAssociatedObject(self, @selector(savedValue), value, OBJC_ASSOCIATION_RETAIN);
+}
+-(NSValue *)savedValue
+{
+    return objc_getAssociatedObject(self, @selector(savedValue));
+}
+-(void)savePosition
+{
+    if(!self.savedValue)
+        self.savedValue = [NSValue valueWithCGPoint:self.position];
+
+}
+-(void)restorePosition
+{
+    NSValue *value = self.savedValue;
+    if(!value) return;
+
+    self.position = value.CGPointValue;
+    self.savedValue = nil;
+}
+
+@end
  ```

  </details>
- 🟢 `Cylinder/tweak/Cylinder-Bridging-Header.h` (added) — +6 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,6 @@
+//
+//  Use this file to import your target's public headers that you would like to expose to Swift.
+//
+
+#import "CylinderAnimator.h"
+#import "CALayer+Cylinder.h"
\ No newline at end of file
  ```

  </details>
- 🟢 `Cylinder/tweak/CylinderAnimator.h` (added) — +51 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,51 @@
+#import <UIKit/UIKit.h>
+
+typedef void (*ObjcMsgSendType)(_Nonnull id, _Nonnull SEL, UIView * _Nonnull, CGFloat);
+
+
+NS_ASSUME_NONNULL_BEGIN
+
+@interface CylinderAnimator : NSObject
+
+@property (nonatomic, assign) bool enabled;
+
+-(instancetype)initWithMsgSend:(ObjcMsgSendType)msgSend;
+-(void)reloadPrefs;
+- (void)manipulate:(nonnull UIView *)view offset:(CGFloat)offset rand:(uint32_t)rand;
+
+@end
+
+@interface SBIcon : NSObject
+@property (nonatomic,copy,readonly) NSString * displayName; 
+@end 
+
+@interface SBIconImageView : UIView
+@end 
+
+@interface SBIconImageCrossfadeView : UIView
+@end 
+
+@interface SBIconView : UIView {
+    SBIconImageView* _iconImageView;
+    SBIconImageCrossfadeView* _crossfadeView;
+}
+
+@property (nonatomic,retain) SBIcon *icon;
+@end 
+
+
+NS_CLASS_AVAILABLE_IOS(4_0) @interface SBIconListView : UIView
+@property(readonly, nonatomic) NSUInteger maximumIconCount;
+@property(readonly, nonatomic) NSUInteger maxIcons API_DEPRECATED_WITH_REPLACEMENT("maximumIconCount", ios(4.0, 13.0));
+@property(readonly, nonatomic) NSUInteger iconColumnsForCurrentOrientation;
+@property(readonly, nonatomic) NSUInteger iconRowsForCurrentOrientation;
+@property (assign, nonatomic) BOOL wasModifiedByCylinder;
+
+-(NSArray *)icons;
+-(void)layoutIconsNow;
+- (void)setIconsNeedLayout;
+- (void)setAlphaForAllIcons:(CGFloat)alpha;
+- (void)enumerateIconViewsUsingBlock:(void (^)(SBIconView* icon, NSUInteger idx, BOOL *stop))block;
+
+@end
+NS_ASSUME_NONNULL_END
\ No newline at end of file
  ```

  </details>
- 🟢 `Cylinder/tweak/CylinderAnimator.swift` (added) — +599 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,599 @@
+//
+//  CylinderAnimator.swift
+//  Cask
+//
+//  Created by Ryan Nair on 2/16/24.
+//
+
+import UIKit
+
+private extension UIView {
+
+    static let perspectiveDistance = (UIScreen.main.bounds.size.width + UIScreen.main.bounds.size.height) / 2
+
+    func rotate(_ angle: CGFloat) {
+        layer.transform = CATransform3DRotate(layer.transform, angle, 0, 0, 1)
+    }
+
+    func rotate(_ angle: CGFloat, _ pitch: CGFloat, _ yaw: CGFloat, _ roll: CGFloat = 1) {
+        var transform = layer.transform
+
+        if !pitch.isZero || !yaw.isZero {
+            transform.m34 = -1/UIView.perspectiveDistance
+        }
+
+        layer.transform = CATransform3DRotate(transform, angle, pitch, yaw, roll)
+    }
+
+    func scale(_ percent: CGFloat) {
+        var transform = layer.transform
+        
+        let oldm34 = transform.m34
+        transform.m34 = -1/UIView.perspectiveDistance
+        transform = CATransform3DScale(transform, percent, percent, 1)
+        transform.m34 = oldm34
+
+        layer.transform = transform
+    }
+
+    func translate(_ x: CGFloat, _ y: CGFloat) {
+        layer.transform = CATransform3DTranslate(layer.transform, x, y, 0)
+    }
+
+    func translate(_ x: CGFloat, _ y: CGFloat, _ z: CGFloat) {
+        var transform = layer.transform
+
+        let oldm34 = transform.m34
+        transform.m34 = -1/UIView.perspectiveDistance
+        transform = CATransform3DTranslate(transform, x, y, z)
+        transform.m34 = oldm34
+
+        layer.transform = transform
+    }
+
+    var width: CGFloat {
+        return frame.size.width / layer.transform.m11
+    }
+     
+    var height: CGFloat {
+        return frame.size.height / layer.transform.m22
+    }
+}
+
+@objc @implementation extension CylinderAnimator { 
+    
+    final private lazy var screenWidth: CGFloat = {
+        return UIScreen.main.bounds.size.width
+    }()
+
+    final private lazy var screenHeight: CGFloat = {
+        return UIScreen.main.bounds.size.height
+    }()
+    
+    var enabled: Bool = false
+
+    final private var msgSend: ObjcMsgSendType
+    final private var scriptSelectors: [Selector] = []
+    final private var scriptCount: UInt32 = 0
+    final private var randomize: Bool = false
+
+    init(msgSend: ObjcMsgSendType) {
+        self.msgSend = msgSend
+        super.init()
+        reloadPrefs()
+    }
+
+    func reloadPrefs() {
+        let settings = UserDefaults(suiteName: "com.ryannair05.cylinderremade")
+        settings?.register(defaults: [
+            "enabled": true,
+            "randomized": false,
+            "effect": [["effect": "Cube (inside)", "effectSelector": "cubeInside"]]
+        ])
+
+        enabled = settings?.bool(forKey: "enabled") ?? true
+        randomize = settings?.bool(forKey: "randomized") ?? false
+
+        if !enabled {
+            scriptSelectors.removeAll()
+            return
+        }
+
+        if let scripts = settings?.array(forKey: "effect") as? [[String: String]] {
+            scriptSelectors.removeAll(keepingCapacity: true)
+
+            scriptSelectors = scripts.compactMap { scriptDict in
+                if let script = scriptDict["effectSelector"] {
+                    return Selector("\(script)::")
+                }
+                return nil
+            }
+
+            scriptCount = UInt32(scriptSelectors.count)
+        }
+    }
+
+    func manipulate(_ view: UIView, offset: CGFloat, rand: UInt32) {
+        if randomize {
+            let scriptSelector = scriptSelectors[Int(rand % scriptCount)]
+            self.msgSend(self, scriptSelector, view, offset)
+        } else {
+            for selector in scriptSelectors {
+                self.msgSend(self, selector, view, offset)
+            }
+        }
+    }
+
+    private func backwards(_ page: UIView, _ offset: CGFloat) {
+        page.translate(2*offset, 0)
+    }
+
+    private func cardHorizontal(_ page: UIView, _ offset: CGFloat) {
+        page.layer.savePosition()
+        page.layer.position.x += offset
+
+        let percent = offset/page.width
+        
+        if abs(percent) >= 0.5 {
+            page.alpha = 0
+        }
+        
+        page.rotate(-.pi * percent, 0, 1, 0)
+    }
+
+    private func cardVertical(_ page: UIView, _ offset: CGFloat) {
+        page.layer.savePosition()
+        page.layer.position.x += offset
+
+        let percent = offset/page.width
+        
+        if abs(percent) >= 0.5 {
+            page.alpha = 0
+        }
+        
+        page.rotate(.pi * percent, 1, 0, 0)
+    }
+    
+    private func chomp(_ page: UIView, _ offset: CGFloat) {
+        var percent = offset/page.width
+        
+        if percent < 0 {
+            percent = -percent
+        }
+        
+        percent *= screenHeight/2
+        
+        for icon in page.subviews {
+            if icon.frame.midY < page.width/2 {
+                page.translate(0, -percent)
+            }
+            else {
+                page.translate(0, percent)
+            }
+        }
+    }
+    
+    private func cubeInside(_ page: UIView, _ offset: CGFloat) {
+        let percent = offset/page.width
+        page.layer.savePosition()
+        page.layer.position.x += offset
+        
+        let angle = percent * .pi/2
+        
+        let h = page.width/2
+        var x = h*cos(abs(angle)) - h
+        let z = h*sin(abs(angle))
+        
+        if percent > 0 {
+            x = -x
+        }
+        
+        x -= offset
+        
+        page.translate(x, 0, z)
+        page.rotate(angle, 0, 1, 0)
+    }
+    
+    private func cubeOutside(_ page: UIView, _ offset: CGFloat) {
+        let percent = offset/page.width
+        page.layer.savePosition()
+        page.layer.position.x += offset
+        
+        var angle = -percent * .pi/2
+        
+        let h = page.width/2
+        var x = h*cos(abs(angle)) - h
+        let z = -h*sin(abs(angle))
+        
+        if percent > 0 {
+            x = -x
+        }
+        
+        x -= offset
+        
+        page.translate(x, 0, z)
+        page.rotate(angle, 0, 1, 0)
+
+        let threshold = abs(atan((UIView.perspectiveDistance - z)/x))
+        angle = abs(angle)
+        
+        if angle > threshold {
+            page.alpha = 1 - (angle - threshold)/(CGFloat.pi/2 - threshold)
+        }
+        else {
+            page.alpha = 1
+        }
+    }
+
+    private func doubleDoor(_ page: UIView, _ offset: CGFloat) {
+        page.translate(offset, 0)
+        
+        let percent = abs(offset/page.width)
+        
+        for icon in page.subviews {
+            if icon.frame.midX > page.width/2 {
+                icon.translate(percent*page.width, 0)
+            }
+            else {
+                icon.translate(-percent*page.width, 0)
+            }
+        }
+    }
+
+    private func doubleHelix(_ page: SBIconListView, _ offset: CGFloat) {
+        let pageWidth = page.width
+        let pageHeight = page.height
+        let maxColumns = CGFloat(page.iconColumnsForCurrentOrientation)
+        let percent = abs(offset / pageWidth)
+        let ops = offset / pageWidth
+        let midx = pageWidth / 2
+        let midy = pageHeight / 2 + 7
+        let fx = min(max(percent * 5, -1), 1)
+        let pc = percent + 0.0001 // Prevent division by zero
+
+        page.enumerateIconViews { icon, i, _ in
+            let iconFrame = icon.center
+            let icx = iconFrame.x
+            let icy = iconFrame.y
+            let ox = midx - icx
+            let oy = midy - icy
+
+            // Calculate the new position
+            let nx = midx - ops / pc * (pageWidth / (7.5 - maxColumns)) *
+                sin(ops * 4 * .pi + 8 * (oy - (1 / maxColumns * ox)) / 1.33 / pageHeight)
+            let ny = midy - oy + (1 / maxColumns) * ox
+
+            // Prevent overlapping by slightly adjusting positions based on index
+            let offsetVal = CGFloat(i + 1) * 0.1
+            let finalNx = nx + offsetVal
+            let finalNy = ny + offsetVal
+
+            // Calculate translation and rotation
+            let translateX = fx * (finalNx - icx)
+            let translateY = fx * (finalNy - icy)
+            let rotateAngle = fx * (ops * 4 * .pi + 0.5 * ops / pc * .pi +
+                8 * (oy - (1 / maxColumns * ox)) / 1.33 / pageHeight)
+
+            icon.translate(translateX, translateY, 0)
+            icon.rotate(rotateAngle, 0, 1, 0)
+        }
+    }
+
+    private func hinge(_ page: UIView, _ offset: CGFloat) {
+        let percent = offset/page.width
+        page.layer.savePosition()
+        page.layer.position.x += offset
+        
+        let angle = percent * .pi
+        var x = page.width/2
+        if percent > 0 {
+            x = -x
+        }
+        
+        page.translate(x, 0)
+        page.rotate(angle, 0, 1, 0)
+        page.translate(-x, 0)
+    }
+
+    private func horizontalAntLines(_ page: UIView, _ offset: CGFloat) {
+         let percent = offset/page.width
+         
+         page.translate(offset, 0)
+         page.alpha = 1 - abs(percent)
+         
+         var direction: CGFloat = 1
+         var lastY: CGFloat = 0
+         
+         for icon in page.subviews {
+             if icon.frame.origin.y > lastY  {
+                 direction = -direction
+                 lastY = icon.frame.origin.y
+             }
+             icon.translate(direction*offset, 0)
+         }
+     }
+
+    private func hyperspace(_ page: SBIconListView, _ offset: CGFloat) {
+        let percent = abs(offset/page.width)
+        let rollup = min(percent * 5, 1)
+        let front: CGFloat = (offset > 0) ? 1 : -1
+        let runaway = max(min((percent-0.2)/0.7, 1), 0)
+        
+        let middleX = page.width/2
+        let middleY = page.height/2 + 7
+
+        page.enumerateIconViews { icon, _ , _ in
+            let iconX = icon.frame.midX
+            let iconY = icon.frame.midY
+
+            let angle = atan((middleY-iconY)/(middleX-iconX))
+            let side: CGFloat = (middleX < iconX) ? -1 : 1
+            let pitch = CGFloat.pi/2.4
+
+            if abs(angle) == CGFloat.pi/2 {
+                let side2: CGFloat = (middleX-iconY > 0) ? -1 : 1
+                icon.rotate(rollup*pitch*side2, 1, 0)
+                icon.translate(-500*runaway*side2*front, 0)
+            }
+            else {
+                icon.rotate(rollup*angle)
+            }
+            icon.rotate(rollup*pitch*side, 0, 1, 0)
+            icon.translate(500*runaway*side*front, 0)
+            icon.alpha = 1 - runaway
+        }
+
+        page.translate(offset, 0)
+    }
+
+    private func leftStairs(_ page: UIView, _ offset: CGFloat) {
+        let percent = offset/page.width
+        let x = percent * -20
+        let z = percent * -100
+        
+        page.translate(x, 0, z)
+    }
+
+    private func iconCollection(_ page: SBIconListView, _ offset: CGFloat) {
+        let percent = abs(offset/page.width)
+        let centerX = page.width/2
+        let centerY = page.height/2
+
+        page.enumerateIconViews { icon, _ , _ in
+            let x = icon.frame.midX
+            let y = icon.frame.midY
+            
+            var hypotenuse = percent*hypot(x-centerX, y-centerY)
+            let angle = atan((centerX - x) / (centerY - y))
+            if y > centerY {
+                hypotenuse = -hypotenuse
+            }
+            
+            let dx = hypotenuse * sin(angle)
+            let dy = hypotenuse * cos(angle)
+            
+            icon.translate(dx, dy)
+        }
+    }
+    
+    private func pageFade(_ page: UIView, _ offset: CGFloat) {
+        let percent = 1 - abs(offset/page.layer.bounds.size.width)
+        
+        page.alpha = percent
+        
+        for icon in page.subviews {
+            icon.alpha = percent
+        }
+    }
+
+    private func pageFlip(_ page: UIView, _ offset: CGFloat) {
+        let percent = offset/page.width
+        
+        let angle = percent * .pi
+        
+        page.alpha = 1 - abs(percent)
+        page.rotate(angle, 0, 1, 0)
+    }
+
+    private func pageTwist(_ page: UIView, _ offset: CGFloat) {
+        let percent = offset/page.width
+        
+        let angle = percent * .pi
+        
+        page.alpha = 1 - abs(percent)
+        page.rotate(-2/3*angle, 1, 0, 0)
+    }
+
+    private func psychospiral(_ page: SBIconListView, _ offset: CGFloat) {
+        let percent = abs(offset/page.layer.bounds.size.width)
+        let side: CGFloat = (offset > 0) ? 1 : -1
+
+        let rollup = min(percent*5, 1)
+        let runaway = max(min((percent-0.20)/0.6, 1), 0)
+
+        let middleX = page.width/2
+        let middleY = page.height/2 + 7
+        let radiusParts = middleY / CGFloat(page.subviews.count)
+        let theta = (3.5*CGFloat.pi) / CGFloat(page.subviews.count)
+
+        page.enumerateIconViews { icon, i , _ in
+            let initAngle = CGFloat(i) * theta
+            let initRadius = CGFloat(i) * radiusParts
+            let angle = initAngle+runaway*(3.5*CGFloat.pi-initAngle)
+            let radius = initRadius+runaway*(middleY-initRadius) + 45
+            let iconX = icon.frame.midX
+            let iconY = icon.frame.midY
+            let pathX = middleX + (radius*cos(angle)) * side
+            let pathY = middleY + (radius*sin(angle)) * side
+
+            icon.translate(rollup*(pathX-iconX), rollup*(pathY-iconY))
+            icon.rotate(rollup * angle)
+            let size = min(1-(1-(radius/(middleY+50)))*rollup, 1)
+            icon.scale(size*size)
+        }
+
+        page.translate(offset, 0)
+    }
+    
+    private func rightStairs(_ page: UIView, _ offset: CGFloat) {
+        let percent = offset/page.width
+        let x = percent * -20
+        let z = percent * 100
+        
+        page.translate(x, 0, z)
+    }
+    
+    private func scatter(_ page: SBIconListView, _ offset: CGFloat) {
+        let percent = abs(offset/page.width)
+
+        page.enumerateIconViews { icon, i , _ in
+            if i % 2 == 1 {
+                icon.translate(0, percent*page.height/2)
+            }
+            else {
+                icon.translate(0, -percent*page.height/2)
+            }
+        }
+    }
+
+    private func shrink(_ page: UIView, _ offset: CGFloat) {
+        let percent = 1 - abs(offset/page.layer.bounds.size.width)
+        
+        for icon in page.subviews {
+            icon.scale(percent)
+        }
+    }
+    
+    private func spin(_ page: UIView, _ offset: CGFloat) {
+        let percent = offset/page.width
+        let angle = percent * .pi * 2
+        
+        for icon in page.subviews {
+            icon.rotate(angle)
+        }
+    }
+    
+    private func suck(_ page: UIView, _ offset: CGFloat) {
+        let percent = offset/page.width
+        let fixed = abs(percent)
+        let side: CGFloat = (percent > 0) ? 1 : 0
+        
+        for icon in page.subviews {
+            let iconX = icon.frame.midX
+            let iconY = icon.frame.midY
+            let absX = iconX+side*(screenWidth-2*iconX)
+            let pathX = page.width * side
+            let pathY = page.height + 7 + icon.height/2
+            let iconAngle = atan(iconY/absX)
+            
+            icon.translate((pathX-iconX)*fixed, (pathY-iconY)*fixed)
+            icon.rotate(percent*iconAngle)
+            icon.scale(sqrt(-fixed+1))
+        }
+    }
+
+    private func verticalAntLines(_ page: UIView, _ offset: CGFloat) {
+        let percent = offset/page.width
+        
+        page.translate(offset, 0)
+        page.alpha = 1 - abs(percent)
+        
+        var direction: CGFloat = 1
+        var lastX = page.width
+        
+        for icon in page.subviews {
+            if lastX > icon.frame.origin.x {
+                direction = -1
+            }
+            else {
+                direction = -direction
+            }
+            
+            lastX = icon.frame.origin.x
+            
+            icon.translate(0, direction*percent*page.height)
+        }
+    }
+
+    private func verticalScrolling(_ page: UIView, _ offset: CGFloat) {
+        let percent = offset/page.width
+        page.translate(offset, percent*page.height)
+        page.alpha = 1 - abs(percent)
+    }
+    
+    private func vortex(_ page: UIView, _ offset: CGFloat) {
+        let percent = abs(offset/page.width)
+        
+        let centerX = page.width/2
+        let centerY = page.height/2 + 7
+        var radius = 0.60 * centerX
+        if radius > page.height {
+            radius = 0.60 * page.height/2
+        }
+        
+        let theta = (2 * .pi) / CGFloat(page.subviews.count)
+        
+        let stage1P = min(percent*3, 1)
+        let stage2P = max(min((percent*3) - 1, 1), 0)
+        
+        let stage3P = stage2P*(.pi/3)
+        let pi_6 = CGFloat.pi/6
+        
+        for (i, icon) in page.subviews.enumerated() {
+            let iconAngle = theta*CGFloat(i) - pi_6 + stage3P
+            
+            let begX = icon.frame.midX
+            let begY = icon.frame.midY
+            
+            let endX = centerX+radius*cos(iconAngle)
+            let endY = centerY-radius*sin(iconAngle)
+            
+            icon.translate((endX-begX)*stage1P, (endY-begY)*stage1P)
+            icon.rotate(-stage1P*(.pi / 2 + iconAngle))
+        }
+        
+        page.alpha = 1 - stage2P
+        page.translate(offset, 0)
+    }
+    
+    private func wave(_ page: UIView, _ offset: CGFloat) {
+        let percent = abs(offset/page.width)
+        let numIcons = page.subviews.count
+        
+        for (i, icon) in page.subviews.enumerated() {
+            let direction: CGFloat = (offset < 0) ? 1 : -1
+            let iconIndex = CGFloat((offset < 0) ? numIcons - i: i - 1)
+            
+            let curIconPercent = percent - ((0.525 / CGFloat(numIcons)) * iconIndex)
+            
+            if curIconPercent > 0 {
+                let dx = direction*(curIconPercent*pow(3.5, 2))*page.width
+                icon.translate(dx, 0)
+            }
+        }
+        
+        page.translate(offset, 0)
+    }
+
+    private func wheel(_ page: UIView, _ offset: CGFloat) {
+        page.layer.savePosition()
+        page.layer.position.x += offset
+
+        let percent = offset/page.width
+
+        for icon in page.subviews {
+            let iconCenterX = icon.frame.midX
+            let iconCenterY = icon.frame.midY
+            let iconCenterXOffset = page.width/2 - iconCenterX
+            let iconRadius = screenHeight - iconCenterY
+
+            let percent2 = ((offset < 0) ?  iconCenterX : page.width - iconCenterX) / page.width
+
+            let angle = -percent*(1 + percent2*2) * CGFloat.pi/2
+
+            icon.translate(iconCenterXOffset, iconRadius)
+            icon.rotate(angle)
+            icon.translate(-iconCenterXOffset, -iconRadius)
+        }
+    }
+}
  ```

  </details>
- 🟢 `Cylinder/tweak/tweak.h` (added) — +25 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,25 @@
+
+#import "CALayer+Cylinder.h"
+#import "CylinderAnimator.h"
+
+@interface SBIconScrollView : UIScrollView
+@end 
+
+@interface SBFolder : NSObject
+@property (nonatomic, assign, readonly) NSUInteger listCount;
+@end
+
+@interface SBRootFolder : SBFolder
+@end
+
+@interface SBIconController : UIViewController
++ (id)sharedInstance;
+@property (nonatomic, assign, readonly) SBRootFolder *rootFolder;
+@end
+
+@interface SBFolderView : UIView 
+@property (assign,getter=isRotating,nonatomic) BOOL rotating;
+@property (nonatomic,copy,readonly) NSArray * iconListViews API_AVAILABLE(ios(7.0));
+
+- (void)enumerateIconListViewsUsingBlock:(void(^)())block;
+@end 
  ```

  </details>
- 🟢 `Cylinder/tweak/tweak.x` (added) — +219 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,219 @@
+#import "tweak.h"
+#import <objc/runtime.h>
+#import <objc/message.h>
+#import <UIKit/UIKit.h>
+#import "../Defines.h"
+
+static CylinderAnimator* animator;
+static u_int32_t _randSeedForCurrentPage;
+
+static BOOL SBIconListView_wasModifiedByCylinder(SBIconView * self, SEL _cmd) {
+    return [objc_getAssociatedObject(self, @selector(wasModifiedByCylinder)) boolValue];
+}
+
+static void SBIconListView_setWasModifiedByCylinder(SBIconView * self, SEL _cmd, BOOL wasModifiedByCylinder) {
+    objc_setAssociatedObject(
+        self,
+        @selector(wasModifiedByCylinder),
+        @(wasModifiedByCylinder),
+        OBJC_ASSOCIATION_RETAIN_NONATOMIC
+    );
+}
+
+static void reset_icon_layout(__unsafe_unretained SBIconListView *self)
+{
+    self.layer.transform = CATransform3DIdentity;
+    [self.layer restorePosition];
+    self.alpha = 1;
+
+    [self enumerateIconViewsUsingBlock:^(SBIconView *v, NSUInteger idx, BOOL *stop) {
+        v.layer.transform = CATransform3DIdentity;
+    }];
+}
+
+static void page_swipe(SBFolderView *self, SBIconScrollView *scrollView)
+{
+    CGRect eye = {scrollView.contentOffset, scrollView.frame.size};
+
+    for (__unsafe_unretained SBIconListView *view in self.iconListViews) {
+
+        if (view.subviews.count < 1) continue;
+
+        if (view.wasModifiedByCylinder) {
+            reset_icon_layout(view);
+        }
+
+        if (CGRectIntersectsRect(eye, view.frame)) {
+            const float offset = scrollView.contentOffset.x - view.frame.origin.x;
+
+            [animator manipulate:(UIView *)view
+                          offset:offset
+                            rand:_randSeedForCurrentPage];
+
+            view.wasModifiedByCylinder = true;
+        }
+    }
+}
+
+static void end_scroll(__unsafe_unretained SBFolderView *self)
+{
+    [self enumerateIconListViewsUsingBlock:^(SBIconListView *view) {
+        reset_icon_layout(view);
+        [view setAlphaForAllIcons:1];
+        view.wasModifiedByCylinder = false;
+    }];
+}
+
+static void loadPrefs()
+{
+    [animator reloadPrefs];
+}
+
+/*
+ Darwin notification trigger
+ Post:
+ com.yourapp.cylinder.trigger
+*/
+static void triggerCylinder(
+    CFNotificationCenterRef center,
+    void *observer,
+    CFStringRef name,
+    const void *object,
+    CFDictionaryRef userInfo
+)
+{
+    if (!animator || !animator.enabled) return;
+
+    Class SBIconControllerClass = objc_getClass("SBIconController");
+    id controller = ((id (*)(id, SEL))objc_msgSend)(
+        SBIconControllerClass,
+        @selector(sharedInstance)
+    );
+
+    if (!controller) return;
+
+    id rootFolderView = ((id (*)(id, SEL))objc_msgSend)(
+        controller,
+        @selector(_rootFolderView)
+    );
+
+    if (!rootFolderView) return;
+
+    UIScrollView *scrollView = nil;
+
+    if ([rootFolderView respondsToSelector:@selector(scrollView)]) {
+        scrollView = ((id (*)(id, SEL))objc_msgSend)(
+            rootFolderView,
+            @selector(scrollView)
+        );
+    }
+
+    if (!scrollView) return;
+
+    _randSeedForCurrentPage = arc4random();
+
+    page_swipe(rootFolderView, (SBIconScrollView *)scrollView);
+}
+
+%hook SBFolderView
+
+-(void)scrollViewDidScroll:(SBIconScrollView *)scrollView
+{
+    %orig;
+
+    if (!animator.enabled) return;
+
+    page_swipe(self, scrollView);
+}
+
+-(void)scrollViewDidEndDecelerating:(SBIconScrollView *)scrollView
+{
+    %orig;
+
+    if (animator.enabled) {
+        end_scroll(self);
+        _randSeedForCurrentPage = arc4random();
+    }
+}
+
+%end
+
+%group iOS15
+
+%hook SBRootFolderView
+
+- (void)updateVisibleColumnRangeWithTotalLists:(NSUInteger)arg1
+                        iconVisibilityHandling:(NSInteger)arg2
+{
+    return %orig(arg1, 0);
+}
+
+%end
+%end
+
+%group iOS14
+
+%hook SBFolderView
+
+-(void)updateVisibleColumnRangeWithTotalLists:(NSUInteger)arg1
+                              columnsPerList:(NSUInteger)arg2
+                      iconVisibilityHandling:(NSInteger)arg3
+{
+    return %orig(arg1, arg2, 0);
+}
+
+%end
+%end
+
+%ctor
+{
+    %init;
+
+    animator = [[CylinderAnimator alloc]
+        initWithMsgSend:(ObjcMsgSendType)objc_msgSend];
+
+    if (@available(iOS 15, *)) {
+        %init(iOS15);
+    } else {
+        %init(iOS14);
+    }
+
+    CFNotificationCenterRef r =
+        CFNotificationCenterGetDarwinNotifyCenter();
+
+    // prefs reload
+    CFNotificationCenterAddObserver(
+        r,
+        NULL,
+        (CFNotificationCallback)loadPrefs,
+        (CFStringRef)kCylinderSettingsChanged,
+        NULL,
+        CFNotificationSuspensionBehaviorCoalesce
+    );
+
+    // manual trigger
+    CFNotificationCenterAddObserver(
+        r,
+        NULL,
+        triggerCylinder,
+        CFSTR("com.yourapp.cylinder.trigger"),
+        NULL,
+        CFNotificationSuspensionBehaviorDeliverImmediately
+    );
+
+    Class SBIconListViewClass = objc_getClass("SBIconListView");
+
+    class_addMethod(
+        SBIconListViewClass,
+        @selector(wasModifiedByCylinder),
+        (IMP)&SBIconListView_wasModifiedByCylinder,
+        "B@:"
+    );
+
+    class_addMethod(
+        SBIconListViewClass,
+        @selector(setWasModifiedByCylinder:),
+        (IMP)&SBIconListView_setWasModifiedByCylinder,
+        "v@:B"
+    );
+}
\ No newline at end of file
  ```

  </details>
- 🟡 `lara/views/tweaks/TweaksView.swift` (modified) — +2 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -43,6 +43,8 @@ struct TweaksView: View {
                         .disabled(!mgr.sbxready)
                     NavigationLink("JIT Enabler", destination: JitView())
                         .disabled(!mgr.sbxready)
+                    NavigationLink("Cylinder", destination: CylinderView())
+                        .disabled(.!mgr.sbxready)
                     NavigationLink("Custom Overwrite", destination: CustomView(mgr: mgr))
                         .disabled(!mgr.vfsready)
                 }
  ```

  </details>
- 🟢 `lara/views/tweaks/system/CylinderView.swift` (added) — +133 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,133 @@
+import UIKit
+import UniformTypeIdentifiers
+
+VStack(alignment: .leading, spacing: 6) {
+    Text("Cylinder")
+        .font(.largeTitle)
+        .fontWeight(.bold)
+        .foregroundColor(.blue)
+}
+.padding()
+
+Button(action: {
+    CFNotificationCenterPostNotification(
+        CFNotificationCenterGetDarwinNotifyCenter(),
+        CFNotificationName("com.lara.cylinder.trigger" as CFString),
+        nil,
+        nil,
+        true
+    )   
+}) {
+    Text("Run Cylinder")
+        .fontWeight(.semibold)
+        .foregroundColor(.white)
+        .padding()
+        .frame(maxWidth: .infinity)
+        .background(Color.blue)
+        .cornerRadius(12)
+}
+.padding()
+
+@objc class FolderBridge: NSObject, UIDocumentPickerDelegate {
+
+    static let shared = FolderBridge()
+
+    private let key = "folderBookmark"
+    private weak var presenterVC: UIViewController?
+
+    @objc static var sharedFolderURL: URL?
+
+    private override init() {}
+
+    // MARK: - Public API (NO PARAMS)
+
+    @objc func requestFolder() {
+        guard let vc = getTopViewController() else {
+            print("❌ No view controller found")
+            return
+        }
+
+        presenterVC = vc
+
+        let picker = UIDocumentPickerViewController(
+            forOpeningContentTypes: [.folder],
+            asCopy: false
+        )
+
+        picker.delegate = self
+        vc.present(picker, animated: true)
+    }
+
+    // MARK: - Get top VC
+
+    private func getTopViewController() -> UIViewController? {
+        guard let scene = UIApplication.shared.connectedScenes.first as? UIWindowScene,
+              let root = scene.keyWindow?.rootViewController else {
+            return nil
+        }
+
+        var top = root
+        while let presented = top.presentedViewController {
+            top = presented
+        }
+        return top
+    }
+
+    // MARK: - Save bookmark
+
+    func documentPicker(_ controller: UIDocumentPickerViewController,
+                        didPickDocumentsAt urls: [URL]) {
+
+        guard let url = urls.first else { return }
+
+        do {
+            let bookmark = try url.bookmarkData(
+                options: .withSecurityScope,
+                includingResourceValuesForKeys: nil,
+                relativeTo: nil
+            )
+
+            UserDefaults.standard.set(bookmark, forKey: key)
+            _ = resolveFolder(url)
+            if url.startAccessingSecurityScopedResource() {
+                // Save to Shared App Group UserDefaults
+                if let sharedDefaults = UserDefaults(suiteName: "lara.com.themystery.uitweaks") {
+                    sharedDefaults.set(url.path, forKey: "sharedFolderPath")
+                    sharedDefaults.synchronize()
+                }
+            }
+            FolderBridge.sharedFolderURL = url
+        } catch {
+            print("Bookmark error:", error)
+        }
+    }
+
+    // MARK: - Resolve
+
+    @objc func resolveFolder(_ incomingURL: URL? = nil) -> URL? {
+
+        var url: URL?
+        var stale = false
+
+        if let incomingURL = incomingURL {
+            url = incomingURL
+        } else if let data = UserDefaults.standard.data(forKey: key) {
+            url = try? URL(
+                resolvingBookmarkData: data,
+                options: .withSecurityScope,
+                relativeTo: nil,
+                bookmarkDataIsStale: &stale
+            )
+        }
+
+        guard let finalURL = url else { return nil }
+
+        if finalURL.startAccessingSecurityScopedResource() {
+            FolderBridge.sharedFolderURL = finalURL
+        } else {
+            print("❌ Cannot access folder")
+        }
+
+        return finalURL
+    }
+}
\ No newline at end of file
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 224cbde: Update TweaksView.swift
- : 📝 Commit 85b7ba0: Add files via upload
- : 📝 Commit 31cbd3a: Add files via upload
- May 8, 2026: 🔴 Closed by @rooootdev


---

## Comments (2)

### Comment #1 — [@rooootdev](https://github.com/rooootdev) · May 8, 2026

`ID: 4409115465`


did you even test this shit?


---

### Comment #2 — [@rooootdev](https://github.com/rooootdev) · May 8, 2026

`ID: 4409148056`


reopen once you a) tested your shit b) confirmed it works. c) cleaned your shitty code


---

*Exported on 10/14/2026, 3:06:01 PM from [https://github.com/rooootdev/lara/pull/235](https://github.com/rooootdev/lara/pull/235)*
