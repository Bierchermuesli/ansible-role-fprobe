# Ansible fprobe role

Just a nifty role to install fprobe. fprobe sends netflow from a linux host. e.g. where:
- temporary captures or statistics
- packetbeat protocol/encoding not supported
- tcpdump is unhandy/too much data
- coordinated on multiple hosts (for aggregation)

# Install
- you have to choose a tighten tcpdump-a-like filter (because it is a 1:1 sampling)! e.g. `fprobe_filter: "(udp && port 162) or icmp"`
- ansible selects it's default NIC. you can specify one with `fprobe_if: eth0`

See example playbook:
- `ansible-playbook roles/fprobe/fprobe-standalone.yml --limit mysensor.example.com`

# Operating
For start and stop operations you can use: 
- `ansible-playbook  -i hosts roles/fprobe/fprobe-start-enable.yml --limit mysensor.example.com`
- `ansible-playbook  -i hosts roles/fprobe/fprobe-stop-disable.yml --limit mysensor.example.com`

For filter change:
- `ansible-playbook  -i hosts roles/fprobe/fprobe-standalone.yml --limit mysensor.example.com --tags config`

# Uninstall
To git rid of everything you can the deinstall play: 
`ansible-playbook  -i hosts roles/fprobe/fprobe-deinstall.yml --limit mysensor.example.com`

# config caveats
 - fprobe runs as root. chroot does not work well
 - complex tcpdump filters don't work via /etc/default/fprobe -> we write a static initd startup file :-/
