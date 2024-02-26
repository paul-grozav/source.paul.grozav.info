---
layout: page
ptitle: Software Packages
---
You can plot the dependencies of a package using:
{% highlight sh %}
apt install -y debtree &&
debtree --with-suggests curl > out.dot &&
dot -T png -o out.png out.dot
{% endhighlight %}

Other commands:

{% highlight sh %}
# Install a specific version of a package:
apt-get install qemu-system-x86=1:5.2+dfsg-11+deb11u2
{% endhighlight %}

---

## Debian

Without the package management tool (with dpkg):
{% highlight sh %}
# List installed packages.
dpkg-query -l
# Install .deb package (will add a package_name in the list of installed packages).
dpkg -i file.deb
# Remove a package from the list of installed packages.
dpkg -P package_name
{% endhighlight %}

With the package management tool:
{% highlight sh %}
# List installed packages.
apt --installed list
# List available packages matching search pattern.
apt-cache search wxmaxima # List all packages containing the word wxmaxima
# List all available(installable) versions for the given package:
apt-cache madison git
apt list -a git
# Show information about a package.
apt-cache show g++
# Install new package.
apt-get install wxmaxima
# Remove a package from the list of installed packages.
apt-get purge wxmaxima
# Show which packages depend on the given package
apt-cache rdepends libxen-4.8
# Show the dependencies of the given package
apt-cache depends podman
# Fix "dpkg: error processing package some_package (--remove): package is in a very bad inconsistent state; you should reinstall it before attempting a removal"
dpkg --remove --force-remove-reinstreq some_package
# Install specific version of package
apt-get install gparted=0.16.1-1

# Key management
# Keys should be placed in /etc/apt/trusted.gpg.d/
# Repos should be placed in /etc/apt/sources.list.d/
# Shows keys in deprecated : /etc/apt/trusted.gpg
apt-key list
apt-key del "DBA3 6B51 81D0 C816 F630  E889 D980 A174 57F6 FB06"
{% endhighlight %}

## Centos

With the package management tool:
{% highlight sh %}
# List installed packages
rpm -qa
# To search for a package you can run:
yum search mariadb-server
# To get information about a given package run:
yum info mariadb-server
# To install a package run:
yum install mariadb-server
# To remove a package run:
yum remove mariadb-server
{% endhighlight %}
see more here: <a href="https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/ch-yum.html" target="_blank">https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/ch-yum.html</a>

## Arch

{% highlight sh %}
# To install a package and it's dependencies:
pacman -S <package_name>
# Uninstall a package and also remove the packages it needs that are not
# used by other packages:
pacman -Rs <package_name>
# Search for packages:
pacman -Ss <package_name>
# Package info:
pacman -Si <package_name>
{% endhighlight %}
