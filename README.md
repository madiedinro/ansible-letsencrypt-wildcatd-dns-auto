# Ansible role containing complete acme dns challenge for wildcard auto renewal.

Key features:
 - Supports DNS and Standalone mode
 - In standalone mode detect http server
    - if found then wait requests from reverse proxy on docker insternal interface
    - listen on external interface to handle http challenge if not
 - Work in docker
 - Start docecker container with crontab for automatic certificate renewal

Configuration example:

```yaml
- block:
    - name: acme.sh LetsEncrypt role
    include_role:
        name: dr.letsencrypt.wildcard.auto
    vars:
        _r_domain: "{{domain}}"
        _r_subdomains: "{{subdomains}}"
        _r_webroot: "{{dirs.well_known}}"
        _r_wildcard: "{{_ssl_wildcard}}" # standalone nor not
        _r_bind_addr: "{{if_inner}}:{{ports.letsencrypt.0}}"
        _r_cert_root: "{{dirs.certs}}"

    - shell: nginx -t && nginx -s reload || /bin/true
    register: _r_nginx_reload
```

other options founs at `defaults/main.yml` and `vars/main.yml`

# License

The MIT License (MIT)

Copyright (c) 2018 Dmitry Rodin

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
