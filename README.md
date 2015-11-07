# yumreposd

This Ansible role allows configuration of Yum repo files, which generally live under /etc/yum.repos.d/

Each repo defined in the main yum repo hash will be created as an individual file under the destination directory. The role will also execute `yum cache expire-metadata` to force a metadata refresh if there are changes to the configured repos on the managed host.

###Role Variables

 * `yumreposd_repos` - This hash contains the repos to be configured, see 'Repo Hash Format', below.
 * `yumreposd_delete_unmanaged` - This controls whether or not repo files in the desination directory that have not been created by this role will be removed, defaults to `false`
 * `yumreposd_preserve` - Used in conjunction with the above; any entries in this list will NOT be purged by the delete_unmanaged task. You must omit the '.repo' filename extension.
 * `yumreposd_destdir` - The directory where you would like the YUM repo configuration snippets to reside, this is almost always the default: `/etc/yum.repos.d/`
 * `yumreposd_importgpgkeys` - Defines whether the role will also import the configured repo GPG keys (if any) into the RPM key database, defaults to `true`

###Repo Hash Format

The repo snippets are defined as a single hash, `yumrepos_repos`, in the following format:

    yumreposd_repos:
      repo-id:
        name: Descriptive Name
        baseurl: http://yourserver.com/repos/repo-id
        gpgkey: file:///etc/pki/rpm-gpg/YOURSITE-KEY
        gpgcheck: 1
        other_option: goes here
      repo-id-2:
        name: Another repo
         ... etc ...

The hash entries under each repo are iterated over during template generation, so any options that are supported in the YUM repo configuration file can be specified here. _No syntax checking is performed._

###Example Playbook

    - hosts: all
      vars:
        - yumreposd_repos:
            base-os:
              name: Base Operating System Packages Repo
              baseurl: http://repos.yourdomain/repos/base-os/{{ ansible_architecture }}
              gpgcheck: 1
              gpgkey: file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
            custom:
              name: My custom repo
              baseurl: http://repos.yourdomain/repos/custom/
              gpgcheck: 0
        - yumreposd_delete_unmanaged: true
        - yumreposd_preserve:
          - epel
          - epel-testing

      roles:
        - yumreposd

As you can see, putting the configuration settings in your playbook can quickly lead to an unweieldy mess, so it's strongly advised that you put them elsewhere for readability.

###License

MIT

Portions of this code are derived from [Jiri Tyr](https://github.com/jtyr)'s [ansible-yumrepo](https://github.com/picotrading/ansible-yumrepo) role.

###Author Information

Please raise any issues via the [GitHub Issue Tracker](https://github.com/jfautley/ansible-role-yumreposd/issues), pull requests are welcome.

* Jon Fautley 
  * Email: <jon@dead.li>
  * GitHub:
      * [Project Page](https://github.com/jfautley/ansible-role-yumreposd)
      * [Personal Page](https://github.com/jfautley)
  * Twitter: [@filace](https://twitter.com/filace)
