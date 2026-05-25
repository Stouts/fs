Stouts.fs
=========

[![CI](https://github.com/Stouts/Stouts.fs/actions/workflows/ci.yml/badge.svg)](https://github.com/Stouts/Stouts.fs/actions/workflows/ci.yml)

Ansible role which helps you with filesystem tasks:

* create directories
* copy raw files
* deploy templates
* remove files and directories

#### Variables

```yaml
fs_dirs: []           # List of directories to create
fs_global_dirs: []    # Global directories to create (merged with fs_dirs)

fs_copy: []           # List of files/templates to copy
fs_global_copy: []    # Global files to copy (merged with fs_copy)

fs_remove: []         # List of files/directories to remove
fs_global_remove: []  # Global files/directories to remove (merged with fs_remove)
```

Each item in `fs_copy` or `fs_global_copy` may contain:

* `dest` — destination path
* `src` — source file path (for raw copy)
* `template` — template path (for template deployment)
* `content` — file content as string
* `owner` — file owner
* `group` — file group
* `mode` — file permissions
* `force` — whether to force overwrite
* `notify` — handler to notify on change

Use `src` for plain files and `template` for Jinja2 templates. If `template` is set, the role will use the `template` module; otherwise it uses `copy`.

Each item in `fs_remove` or `fs_global_remove` may be a path string or a dict with:

* `path` — path to remove

#### Usage

Add `Stouts.fs` to your roles and set vars in your playbook file.

Example:

```yaml
- hosts: all

  roles:
    - Stouts.fs

  vars:
    fs_dirs:
      - /var/log/myapp
      - /opt/myapp/data

    fs_copy:
      - dest: /etc/myapp/config.yml
        src: files/config.yml

      - dest: /etc/nginx/nginx.conf
        template: templates/nginx.conf.j2
        owner: root
        group: root
        mode: '0644'
        notify: restart nginx

    fs_remove:
      - /tmp/old-stuff
      - /tmp/old-directory
```

#### License

Licensed under the MIT License. See the LICENSE file for details.

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Stouts/Stouts.fs/issues)!
