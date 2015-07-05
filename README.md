Mac provisioning by Homebrew and Ansible
============


Usage
-----

```
# install XCode
sudo xcodebuild -license

# install Homebrew
xcode-select --install
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew doctor
brew update

# install Ansible
brew install python
brew install ansible

# setting Homebrew Cask
echo 'export HOMEBREW_CASK_OPTS="--appdir=/Applications"' >> ~/.bash_profile
source ~/.bash_profile
```

1. clone this repository.
1. edit playbook. list the managed software to variable.
1. run `ansible-playbook -i hosts -vv mac-development.yml`


Example Playbook
----------------

```
- hosts: localhost
  connection: local
  gather_facts: no
  sudo: no
  roles:
    - homebrew
    - homebrew-cask
  vars:
    # Tap external Homebrew repositories.
    #
    # e.g.
    # - homebrew/binary
    homebrew_repositories:

    # Managed Homebrew packages.
    #
    # e.g.
    # - package_name
    # or
    # { name: package_name, state: package_state, install_options: [with-baz, enable-debug] }
    #
    # state choices: [head, latest, present, absent, linked, unlinked] (default: latest)
    # install_options: string or sequence (default: none)
    homebrew_packages:
      - readline
      - openssl
      - { name: openssl, state: linked, install_options: force }
      - ansible
      - name: git
        install_options:
          - with-brewed-curl
          - with-gettext
      - rbenv
      - ruby-build

    # Tap external Homebrew Cask repositories.
    homebrew_cask_repositories:

    # Managed Homebrew Cask packages.
    #
    # e.g.
    # - package_name
    # or
    # { name: package_name, state: package_state }
    #
    # state choices: [present, absent, installed, uninstalled] (default: present)
    homebrew_cask_packages:
      - firefox
      - google-chrome
      - google-japanese-ime
      - intellij-idea
      - karabiner
      - phpstorm
      - slack
      - vagrant
      - virtualbox
```


License
-------

MIT
