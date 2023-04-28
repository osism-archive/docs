# Horizon

## Disable "Consistency Groups"  and "Consistency Group Snapshots" dashboards

The two dashboards **Consistency Groups** and **Consistency Group Snapshots** are unfortunately always activated in Horizon and
there is no configuration parameter to deactivate them. Therefore only the deactivation by manual removal of files within the
Horizon containers works at the moment.

```sh
   $ docker exec -it horizon bash
   (horizon)[root@20-10 /]# find /var/lib/kolla \
     -name '_1340_project_consistency_groups_panel.py*' \
     -exec rm -f {} \;
   (horizon)[root@20-10 /]# find /var/lib/kolla \
     -name '_1350_project_cg_snapshots_panel.py*' \
     -exec rm -f {} \;
```

```sh
docker restart horizon
```

## Horizon webinterface broken

### Description

![horizon-broken](./images/horizon-broken.png)

### Solution

You have to cleanup and restart all horizon containers.

* Up to Queens release:

    ```sh
    docker exec -it horizon rm /var/lib/kolla/.local_settings.md5sum.txt && docker restart horizon
    ```

* As of Rocky release

    ```sh
    docker exec -it horizon rm /var/lib/kolla/.settings.md5sum.txt && docker restart horizon
    ```

## Large Horizon table for django_session

* table django_session size in database horizon is large

    ```sh
    $ ls -lah /var/lib/docker/volumes/mariadb/_data/horizon/
    total 3.5G
    ...
    -rw-rw----  1 42434 42434 1.6K Sep 10 12:07 django_session.frm
    -rw-rw----  1 42434 42434 3.5G Dec  5 14:53 django_session.ibd
    ...
    ```

* cleanup the sessions in horizon container

    ```sh
    docker exec -it horizon manage.py clearsessions
    ```

* optimize the table size

    ```sh
    $ docker exec -it mariadb mysqlcheck -u root -p --optimize --skip-write-binlog horizon django_session
    Enter password:
    horizon.django_session
    note     : Table does not support optimize, doing recreate + analyze instead
    status   : OK
    ```

* table django_session size in database horizon

    ```sh
    $ sudo ls -lah /var/lib/docker/volumes/mariadb/_data/horizon/
    ...
    -rw-rw----  1 42434 42434 1.6K Dec  5 15:02 django_session.frm
    -rw-rw----  1 42434 42434 9.0M Dec  5 15:04 django_session.ibd
    ...
    ```

## Customize horizon theme

From the Upstream Documentation: <https://docs.openstack.org/kolla-ansible/latest/reference/shared-services/horizon-guide.html#extending-the-default-local-settings-options>

* Prepare overlay files, as an example where theme files should placed.

    ```sh
    $ tree -L 2  /opt/configuration/environments/kolla/files/overlays/horizon/

    ├── custom_local_settings
    └── themes
        └── osism
    ```

* to enable it in the kolla/configuration.yml
  
    ```sh
    # horizon

    horizon_custom_themes:
        - name: osism
        label: CustomTheme
    ```

* choose theme as default in kolla/files/overlays/horizon/custom_local_settings

    ```sh
    AVAILABLE_THEMES = [
            ('OSISM', 'osism,', '/etc/openstack-dashboard/themes/osism'),
    ]

    DEFAULT_THEME = 'osism'
    ```

* Changing the Brand Link:
  <https://docs.openstack.org/horizon/zed/configuration/customizing.html#changing-the-brand-link>

* Customize Footer:
  <https://docs.openstack.org/horizon/latest/configuration/customizing.html#customizing-the-footer>
  
    ```sh
    {% extends "base.html" %}

    {% block footer %}
    <p>My custom footer</p>
    {% endblock %} 
    ```

    auth/login.html:

    ```sh
    {% extends "auth/login.html" %}

    {% block footer %}
        <p>customize login footer</p>
    {% endblock %}
    ```

    auth/_login_form.html:

    ```sh
    {% extends "auth/_login_form.html" %}

    {% block login_footer %}
    {% comment %}
        Message of to day login button.
    {% endcomment %}
    {{ block.super }}
            <p>customize login form footer</p>
    {% endblock %}
    ```
