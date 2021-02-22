Role Name
=========

Utility Role to help configure a disconnected OLM in a disconnected/air-gapped OpenShift Container Platform 4.6+ cluster. 

Requirements
------------


Role Variables
--------------

- ansible_name_module: The default name of this module (default to 'config-disconnected-OLM')
- registry_host_fqdn: Host FQDN to use. It will also be used in the cert created and related SAN
- registry_host_port: Port to bind the registry to on the host. Default to 5000
- registry_admin_username: The admin username for the registry. Used for the htpasswd authentication.
- registry_admin_password: The admin password for the registry . Used for the htpasswd authentication.
- openshift_cli: Openshift client binary used to interact with the cluster api (default to 'oc')
- ocp_cluster_console_port: The port on which the cluster api is listening (default to 6443)
- staging_dir: The directory where rendered yaml config are placed on the '/tmp'
- disconnected_operatorhub_config: The name of the processed OLM yaml config (default to 'operatorhub-config.yml')
- disconnected_catalog_source_config: The name of the processed catalag source yaml config (default to 'catalog-source-config.yml')
- operator_image_content_source_file: The name the Image Content Source config for operators images in the disconnected registry. 
- local_repository: The name of repository where the operator images are stored in the disconnected registry (default to 'openshift4/redhat-operators')
- operator_catalogs_to_deploy: The structure containing information about the various operator indices to deploy.These valued as used to generate the catalog source yaml file and the Image Content source file to use for the catalog.
   - catalog_name: The name of the catalog.
   - catalog_index: The name of the operator index in the disconnected registry. 
   - catalog_index_tag: The tag of the operator index in the disconnected registry.
   - catalog_publisher: The name of the catalog publisher.
   - content_source_file: The fully qualified location of the image content source policy file for the catalog
   - mirror: Whether the catalog need to be deployed or not. 


Dependencies
------------

The controller is expected to have the OC client installed as well as git so that the jinja2 templates can be processed. If not there can be processed somewhere else and the rendered yaml files brought to the controller to run the role.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: localhost 
      vars:
        registry_host_fqdn: 'registry.example.com'
        local_repository: 'openshift4/redhat-operators'
        ocp_cluster_user_user: 'registry-example-user'
        ocp_cluster_user_password: 'registry-example-password'
        ocp_cluster_console_url: 'api.ocp-cluster.example.com'
        ocp_cluster_console_port: '6443'
        staging_dir: '/tmp'
        disconnected_operatorhub_config: 'patched-operatorhub.yml'
        operator_image_content_source_file: '/tmp/imageContentSourcePolicy.yaml'
        disconnected_catalog_source_config: 'catalog-source-config.yml'
        operator_catalog_name: 'redhat-operator-catalog'
        operator_catalog_index: 'redhat-operator-index'
        operator_catalog_index_tag: 'v4.6'
        operator_catalog_publisher: 'example-cluster'
        operator_catalogs_to_deploy:
          redhat-operators:
            catalog_name: 'custom-redhat-operator-catalog'
            catalog_index: 'redhat-operator-index'
            catalog_index_tag: 'v4.6'
            catalog_publisher: 'redhat'
            content_source_file: ''  ##'<path/to/imageContentSourcePolicy-artifactory.yaml>'
            mirror: "true"
          community-operators:
            catalog_name: 'community-redhat-operator-catalog'
            catalog_index: 'community-operator-index'
            catalog_index_tag: 'latest'
            catalog_publisher: 'community'
            content_source_file: ''  ##'<path/to/imageContentSourcePolicy-artifactory.yaml>'
            mirror: "true"
      roles:
         - { role: config-disconnected-OLM }

License
-------

BSD

Author Information
------------------

