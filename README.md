###################### Filebeat Configuration #########################

filebeat.inputs:

- type: eventlog
  enabled: true
  event_logs:
    - name: Application
    - name: System
    - name: Security
    - name: Microsoft-Windows-Sysmon/Operational
      tags: ["sysmon"]
    # - name: Microsoft-Windows-PowerShell/Operational
    #   tags: ["powershell"]

filebeat.config.modules:
  path: ${path.config}/modules.d/*.yml
  reload.enabled: false

setup.template.settings:
  index.number_of_shards: 1

setup.kibana:
  host: "http://192.168.1.11:5601"

output.logstash:
  hosts: ["192.168.1.11:5044"]

processors:
  - add_host_metadata:
      when.not.contains.tags: forwarded
  - add_cloud_metadata: ~
  - add_docker_metadata: ~
  - add_kubernetes_metadata: ~

logging.level: info
logging.to_files: true
logging.files:
  path: C:\ProgramData\filebeat\logs

