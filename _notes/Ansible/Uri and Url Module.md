[[Index]] 


# Uri module

```yaml
- name: Check page contents, return status 200 and fail if the page contents uri doesn’t contain the word Linux
  uri:
    url: http://www.example.com
    validate_certs: no
    return_content: yes
  register: this
  failed_when: "'Linux' not in this.content"

``` 

  

# get_url module

```yaml
- hosts: all
  become: yes
  tasks:
    - name: Download File with authentication
      get_url:
        url: http://102.15.192.120/backups/database.tar.gz
        url_username: user
        url_password: '{{ pass }}'
        url_password: '{{ API_TOKEN }}'
        force_basic_auth: yes
        validate_certs: no
        mode: 0777
        timeout: 5
        dest: /backups


```  

