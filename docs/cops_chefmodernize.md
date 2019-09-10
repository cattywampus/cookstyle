# ChefModernize

## ChefModernize/CronManageResource

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

The cron_manage resource was renamed to cron_access in the 6.1 release of the cron
cookbook, and later shipped in Chef Infra Client 14.4. The new resource name should
be used.

  # bad
  cron_manage 'mike'

  # good
  cron_access 'mike'

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.6.0` | String
Exclude | `**/metadata.rb` | Array

## ChefModernize/CustomResourceWithAllowedActions

Enabled by default | Supports autocorrection
--- | ---
Enabled | Yes

In HWRPs and LWRPs it was necessary to define the allowed actions within the resource.
In custom resources this is no longer necessary as Chef will determine it based on the
actions defined in the resource.

### Examples

```ruby
# bad
property :something, String

allowed_actions [:create, :remove]
action :create do
  # some action code because we're in a custom resource
end

# also bad
property :something, String

actions [:create, :remove]
action :create do
  # some action code because we're in a custom resource
end

# good
property :something, String

action :create do
  # some action code because we're in a custom resource
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.2.0` | String
Include | `**/resources/*.rb` | Array

## ChefModernize/CustomResourceWithAttributes

Enabled by default | Supports autocorrection
--- | ---
Enabled | Yes

In HWRPs and LWRPs you defined attributes, but custom resources changed the name to
be properties to avoid confusion with chef recipe attributes. When writing a custom resource
they should be called properties even though the two are aliased.

### Examples

```ruby
# bad
attribute :something, String

action :create do
  # some action code because we're in a custom resource
end

# good
property :something, String

action :create do
  # some action code because we're in a custom resource
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.2.0` | String
Include | `**/resources/*.rb` | Array

## ChefModernize/DefinesChefSpecMatchers

Enabled by default | Supports autocorrection
--- | ---
Enabled | Yes

ChefSpec 7.1 and later auto generate ChefSpec matchers. Matchers in cookbooks can now be removed.

### Examples

```ruby
# bad
if defined?(ChefSpec)
  def create_yum_repository(resource_name)
    ChefSpec::Matchers::ResourceMatcher.new(:yum_repository, :create, resource_name)
  end
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.3.0` | String
Include | `**/libraries/*.rb` | Array

## ChefModernize/DependsOnZypperCookbook

Enabled by default | Supports autocorrection
--- | ---
Enabled | Yes

Don't depend the zypper cookbook as the zypper_repository resource is built into
Chef Infra Client 13.3

### Examples

```ruby
# bad
depends 'zypper'
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.6.0` | String
Exclude | `**/metadata.rb` | Array

## ChefModernize/ExecuteAptUpdate

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

Instead of using the execute resource to to run the `apt-get update` use Chef Infra Client's built-n
apt_update resource which is available in Chef Infra Client 12.7 and later.

  # bad
  execute 'apt-get update'

  # good
  apt_update

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.3.0` | String
Exclude | `**/metadata.rb` | Array

## ChefModernize/ExecuteTzUtil

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

Instead of using the execute or powershell_script resources to to run the `tzutil` command, use
Chef Infra Client's built-in timezone resource which is available in Chef Infra Client 14.6 and later.

  # bad
  execute 'set tz' do
    command 'tzutil.exe /s UTC'
  end

  execute 'tzutil /s UTC'

  powershell_script 'set windows timezone' do
    code "tzutil.exe /s UTC"
    not_if { shell_out('tzutil.exe /g').stdout.include?('UTC') }
  end

  # good
  timezone 'UTC'

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.6.0` | String
Exclude | `**/metadata.rb` | Array

## ChefModernize/IncludingAptDefaultRecipe

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

Don't include the apt default recipe to update apt's package cache when you can
use the apt_update resource built into Chef Infra Client 12.7 and later.

### Examples

```ruby
# bad
include_recipe 'apt::default'
include_recipe 'apt'

# good
apt_update
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.3.0` | String
Exclude | `**/metadata.rb` | Array

## ChefModernize/IncludingMixinShelloutInResources

Enabled by default | Supports autocorrection
--- | ---
Enabled | Yes

There is no need to include Chef::Mixin::ShellOut in resources or providers as this is already done by Chef Infra Client.

### Examples

```ruby
# bad
require 'chef/mixin/shell_out'
include Chef::Mixin::ShellOut
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.4.0` | String
Include | `**/resources/*.rb`, `**/providers/*.rb` | Array

## ChefModernize/IncludingWindowsDefaultRecipe

Enabled by default | Supports autocorrection
--- | ---
Enabled | Yes

Don't include the windows default recipe that is either full of gem install that are part
of the Chef Infra Client, or empty (depends on version).

### Examples

```ruby
# bad
include_recipe 'windows::default'
include_recipe 'windows'
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.3.0` | String
Exclude | `**/metadata.rb` | Array

## ChefModernize/LegacyBerksfileSource

Enabled by default | Supports autocorrection
--- | ---
Enabled | Yes

Over the course of years there have been many different valid community site / Supermarket
URLs to use in a cookbook's Berksfile. These old URLs continue to function via redirects,
but should be updated to point to the latest Supermarket URL.

### Examples

```ruby
# bad
source 'http://community.opscode.com/api/v3'
source 'https://supermarket.getchef.com'
source 'https://api.berkshelf.com'

# good
source 'https://supermarket.chef.io'
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.1.0` | String
Include | `**/Berksfile` | Array

## ChefModernize/LibarchiveFileResource

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

Use the archive_file resource built into Chef Infra Client 15+ instead of the libarchive_file resource

### Examples

```ruby
# bad
libarchive "C:\file.zip" do
  path 'C:\expand_here'
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.5.0` | String
Exclude | `**/metadata.rb` | Array

## ChefModernize/MacOsXUserdefaults

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

The mac_os_x_userdefaults resource was renamed to macos_userdefaults when it was added to Chef Infra Client
14.0. The new resource name should be used.

  # bad
  mac_os_x_userdefaults 'full keyboard access to all controls' do
    domain 'AppleKeyboardUIMode'
    global true
    value '2'
  end

  # good
  macos_userdefaults 'full keyboard access to all controls' do
    domain 'AppleKeyboardUIMode'
    global true
    value '2'
  end

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.6.0` | String
Exclude | `**/metadata.rb` | Array

## ChefModernize/MinitestHandlerUsage

Enabled by default | Supports autocorrection
--- | ---
Enabled | Yes

Use Chef InSpec for testing instead of the Minitest Handler cookbook pattern.

### Examples

```ruby
# bad
depends 'minitest-handler'
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.4.0` | String
Include | `**/metadata.rb` | Array

## ChefModernize/OpensslRsaKeyResource

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

The openssl_rsa_key resource was renamed to openssl_rsa_private_key in Chef
Infra Client 14.0. The new resource name should be used.

  # bad
  openssl_rsa_key '/etc/httpd/ssl/server.key' do
    key_length 2048
  end

  # good
  openssl_rsa_private_key '/etc/httpd/ssl/server.key' do
    key_length 2048
  end

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.6.0` | String
Exclude | `**/metadata.rb` | Array

## ChefModernize/OpensslX509Resource

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

The openssl_x509 resource was renamed to openssl_x509_certificate.
The new resource name should be used.

  # bad
  openssl_x509 '/etc/httpd/ssl/mycert.pem' do
    common_name 'www.f00bar.com'
    org 'Foo Bar'
    org_unit 'Lab'
    country 'US'
  end

  # good
  openssl_x509_certificate '/etc/httpd/ssl/mycert.pem' do
    common_name 'www.f00bar.com'
    org 'Foo Bar'
    org_unit 'Lab'
    country 'US'
  end

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.6.0` | String
Exclude | `**/metadata.rb` | Array

## ChefModernize/OsxConfigProfileResource

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

The osx_config_profile resource was renamed to osx_profile.
The new resource name should be used.

  # bad
  osx_config_profile 'Install screensaver profile' do
    profile 'screensaver/com.company.screensaver.mobileconfig'
  end

  # good
  osx_profile 'Install screensaver profile' do
    profile 'screensaver/com.company.screensaver.mobileconfig'
  end

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.6.0` | String
Exclude | `**/metadata.rb` | Array

## ChefModernize/PowershellInstallPackage

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

Use the powershell_package resource built into Chef Infra Client instead of the powershell_script
resource to run Install-Package

 # good
 powershell_package 'docker'

### Examples

```ruby
# bad
powershell_script 'Expand website' do
  code 'Install-Package -Name docker'
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.5.0` | String
Exclude | `**/metadata.rb` | Array

## ChefModernize/PowershellInstallWindowsFeature

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

Use the windows_feature resource built into Chef Infra Client 15+ instead of the powershell_script resource
to run Install-WindowsFeature or Add-WindowsFeature

 # good
 windows_feature 'Net-framework-Core' do
   action :install
   install_method :windows_feature_powershell
 end

### Examples

```ruby
# bad
powershell_script 'Install Feature' do
  code 'Install-WindowsFeature -Name "Net-framework-Core"'
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.5.0` | String
Exclude | `**/metadata.rb` | Array

## ChefModernize/PowershellScriptExpandArchive

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

Use the archive_file resource built into Chef Infra Client 15+ instead of using the powershell_script
resource to run Expand-Archive

### Examples

```ruby
# bad
powershell_script 'Expand website' do
  code 'Expand-Archive "C:\\file.zip" -DestinationPath "C:\\inetpub\\wwwroot\\" -Force'
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.5.0` | String
Exclude | `**/metadata.rb` | Array

## ChefModernize/RespondToInMetadata

Enabled by default | Supports autocorrection
--- | ---
Enabled | Yes

It is not longer necessary respond_to?(:foo) in metadata. This was used to support new metadata
methods in Chef 11 and early versions of Chef 12.

### Examples

```ruby
# bad
chef_version '>= 13' if respond_to?(:chef_version)

# good
chef_version '>= 13'
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.2.0` | String
Include | `**/metadata.rb` | Array

## ChefModernize/RespondToProvides

Enabled by default | Supports autocorrection
--- | ---
Enabled | Yes

It is not longer necessary respond_to?(:foo) in metadata. This was used to support new metadata
methods in Chef 11 and early versions of Chef 12.

### Examples

```ruby
# bad
provides :foo if respond_to?(:provides)

# good
provides :foo
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.2.0` | String
Include | `**/providers/*.rb`, `**/libraries/*.rb` | Array

## ChefModernize/RespondToResourceName

Enabled by default | Supports autocorrection
--- | ---
Enabled | Yes

Chef 12.5 introduced the resource_name method for resources. Many cookbooks used
respond_to?(:resource_name) to provide backwards compatibility with older chef-client
releases. This backwards compatibility is no longer necessary.

### Examples

```ruby
# bad
resource_name :foo if respond_to?(:resource_name)

# good
resource_name :foo
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.2.0` | String
Include | `**/resources/*.rb`, `**/libraries/*.rb` | Array

## ChefModernize/SetOrReturnInResources

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

set_or_return within a method should not be used to define property in a resource. Instead use
the property method which properly validates and defines properties in a way that works with
reporting and documentation functionality in Chef Infra Client

### Examples

```ruby
# bad
 def severity(arg = nil)
   set_or_return(
     :severity, arg,
     :kind_of => String,
     :default => nil
   )
 end

# good
property :severity, String
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.2.0` | String
Include | `**/resources/*.rb`, `**/libraries/*.rb` | Array

## ChefModernize/SevenZipArchiveResource

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

Use the archive_file resource built into Chef Infra Client 15+ instead of the seven_zip_archive

### Examples

```ruby
# bad
seven_zip_archive "C:\file.zip" do
  path 'C:\expand_here'
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.5.0` | String
Exclude | `**/metadata.rb` | Array

## ChefModernize/ShellOutToChocolatey

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

Use the Chocolatey resources built into Chef Infra Client instead of shelling out to the choco command

 powershell_script 'add artifactory choco source' do
   code "choco source add -n=artifactory -s='https://mycorp.jfrog.io/mycorp/api/nuget/chocolatey-remote' -u foo -p bar"x
   not_if 'choco source list | findstr artifactory'
 end

### Examples

```ruby
# bad
execute 'install package foo' do
  command "choco install --source=artifactory \"foo\" -y --no-progress --ignore-package-exit-codes"
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.5.0` | String
Exclude | `**/metadata.rb` | Array

## ChefModernize/SysctlParamResource

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

The sysctl_param resource was renamed to sysctl when it was added to Chef Infra Client
14.0. The new resource name should be used.

  # bad
  sysctl_param 'fs.aio-max-nr' do
    value '1048576'
  end

  # good
  sysctl 'fs.aio-max-nr' do
    value '1048576'
  end

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.6.0` | String
Exclude | `**/metadata.rb` | Array

## ChefModernize/UnnecessaryDependsChef14

Enabled by default | Supports autocorrection
--- | ---
Enabled | Yes

Don't depend on cookbooks made obsolete by Chef 14

### Examples

```ruby
# bad
depends 'build-essential'
depends 'chef_handler'
depends 'chef_hostname'
depends 'dmg'
depends 'mac_os_x'
depends 'swap'
depends 'sysctl'
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.1.0` | String
Include | `**/metadata.rb` | Array

## ChefModernize/UseBuildEssentialResource

Enabled by default | Supports autocorrection
--- | ---
Enabled | Yes

Use secure Github and Gitlab URLs for source_url and issues_url

### Examples

```ruby
# bad
depends 'build-essential'
include_recipe 'build-essential::default'
include_recipe 'build-essential'

# good
build_essential 'install compilation tools'
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.1.0` | String
Exclude | `**/metadata.rb` | Array

## ChefModernize/UsesZypperRepo

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

The zypper_repo resource was renamed zypper_repository when it was added to Chef Infra Client 13.3.

### Examples

```ruby
# bad
zypper_repo 'apache' do
  baseurl 'http://download.opensuse.org/repositories/Apache'
  path '/openSUSE_Leap_42.2'
  type 'rpm-md'
  priority '100'
end

# good
zypper_repository 'apache' do
  baseurl 'http://download.opensuse.org/repositories/Apache'
  path '/openSUSE_Leap_42.2'
  type 'rpm-md'
  priority '100'
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.6.0` | String
Exclude | `**/metadata.rb` | Array

## ChefModernize/WhyRunSupportedTrue

Enabled by default | Supports autocorrection
--- | ---
Enabled | Yes

whyrun_supported? no longer needs to be set to true as that is the default in Chef Infra Client 13+

### Examples

```ruby
# bad
def whyrun_supported?
 true
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.1.0` | String
Include | `**/resources/*.rb`, `**/providers/*.rb`, `**/libraries/*.rb` | Array

## ChefModernize/WindowsVersionHelper

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

Use node['platform_version] data instead of the Windows::VersionHelper helper from the Windows cookbook.

### Examples

```ruby
# bad
Windows::VersionHelper.nt_version

# good
node['platform_version].to_i
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.4.0` | String
Exclude | `**/metadata.rb` | Array

## ChefModernize/WindowsZipfileUsage

Enabled by default | Supports autocorrection
--- | ---
Enabled | No

Use the archive_file resource built into Chef Infra Client 15+ instead of the windows_zipfile from the Windows cookbook

### Examples

```ruby
# bad
windows_zipfile 'C:\\files\\' do
  source 'C:\\Temp\\file.zip'
end
```

### Configurable attributes

Name | Default value | Configurable values
--- | --- | ---
VersionAdded | `5.4.0` | String
Exclude | `**/metadata.rb` | Array