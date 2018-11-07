# frozen_string_literal: true

source 'https://github.com/CocoaPods/Specs.git'

# Default platform for most targets
platform :ios, '10.0'

# Ignore all warnings from all pods
inhibit_all_warnings!

workspace 'PodFrameworksIssue.xcworkspace'

target 'AppTarget' do
  project 'AppTarget/AppTarget.xcodeproj'

  pod 'ObjectiveRocksFramework'

  target 'AppTargetUITests' do
    inherit! :search_paths
  end
end
