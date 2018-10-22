+++
title = "Passo 3 - Ad-Hoc"
description = "Primeiros Comandos Ad-Hoc"
weight = 4
+++
## Primeiros comandos Ad-Hoc:
Após a conclusão da instalação, o Ansible já estará pronto para iniciar os primeiros comandos de automação.

#### Verificando a versão do Ansible

```bash
$ ansible --version

ansible 2.3.2.0
	config file = /etc/ansible/ansible.cfg
	configured module search path = Default w/o overrides
	python version = 2.7.13 (default, May 10 2017, 20:04:36)
	[GCC 6.3.1 20161221 (Red Hat 6.3.1-1)]
```

#### [MÓDULO PING](http://docs.ansible.com/ansible/latest/ping_module.html): Verificar conectividade entre o Ansible Control e o Host

```bash
$ ansible localhost -m ping

[WARNING]: Host file not found: /etc/ansible/hosts
[WARNING]: provided hosts list is empty, only localhost is available
localhost | SUCCESS => {
	"changed": false,
	"ping": "pong"
}
```

{{% notice tip %}}
Quando nenhum arquivo de inventário é definido (Por padrão /etc/ansible/host ou o parâmetro -i <*path/nomeinventario*>), somente o *Host* 'localhost' fica disponível para os comandos Ansible. Para *Hosts* remotos será necessário definí-los num arquivo de inventário.
{{% /notice%}}

{{% notice info %}}
O módulo 'ping' foi informado através do parâmetro `-m` e, no caso de sucesso, retorna 'pong'. A exibição dos resultados dos comandos Ansible é feita no formato json, mas [pode ser alterada para yaml a partir da versão 2.5](https://www.jeffgeerling.com/blog/2018/use-ansibles-yaml-callback-plugin-better-cli-experience).
{{% /notice%}}

{{% notice info %}}
O comando ping não envia pacotes ICMP para o *Host*. Este módulo realiza um teste de verificação de permissão, da instalação do python **Host** e que este está acessível pelo Ansible Control.

{{% /notice%}}

#### [MÓDULO SETUP](http://docs.ansible.com/ansible/latest/setup_module.html): Coletando informações do host (Gathers facts)

{{% expand "Clique aqui para expandir:" %}}
```bash
$ ansible localhost -m setup

localhost | SUCCESS => {
		"ansible_facts": {
				"ansible_all_ipv4_addresses": [
						"192.168.0.4",
						"192.168.33.1"
				],
				"ansible_all_ipv6_addresses": [
						"fe80::cf0:a4b6:79f1:9155%en1",
						"2804:14c:65d6:5af0:1cba:e7e2:6423:1848",
						"2804:14c:65d6:5af0:e8e7:20ff:caeb:ea6d",
						"fe80::b5ba:39ce:b960:939d%utun0"
				],
				"ansible_apparmor": {
						"status": "disabled"
				},
				"ansible_architecture": "x86_64",
				"ansible_bridge0": {
						"device": "bridge0",
						"flags": [
								"UP",
								"BROADCAST",
								"SMART",
								"RUNNING",
								"SIMPLEX",
								"MULTICAST"
						],
						"ipv4": [],
						"ipv6": [],
						"macaddress": "d2:00:18:4a:a4:00",
						"media": "Unknown",
						"media_select": "Unknown",
						"media_type": "unknown type",
						"mtu": "1500",
						"options": [
								"PERFORMNUD",
								"DAD"
						],
						"status": "inactive",
						"type": "ether"
				},
				"ansible_date_time": {
						"date": "2017-09-16",
						"day": "16",
						"epoch": "1505599934",
						"hour": "19",
						"iso8601": "2017-09-16T22:12:14Z",
						"iso8601_basic": "20170916T191214896080",
						"iso8601_basic_short": "20170916T191214",
						"iso8601_micro": "2017-09-16T22:12:14.896212Z",
						"minute": "12",
						"month": "09",
						"second": "14",
						"time": "19:12:14",
						"tz": "-03",
						"tz_offset": "-0300",
						"weekday": "Saturday",
						"weekday_number": "6",
						"weeknumber": "37",
						"year": "2017"
				},
				"ansible_default_ipv4": {
						"address": "192.168.0.4",
						"broadcast": "192.168.0.255",
						"device": "en1",
						"flags": [
								"UP",
								"BROADCAST",
								"SMART",
								"RUNNING",
								"SIMPLEX",
								"MULTICAST"
						],
						"gateway": "192.168.0.1",
						"interface": "en1",
						"macaddress": "b8:8d:12:3e:a5:66",
						"media": "Unknown",
						"media_select": "autoselect",
						"mtu": "1500",
						"netmask": "255.255.255.0",
						"network": "192.168.0.0",
						"options": [
								"PERFORMNUD",
								"DAD"
						],
						"status": "active",
						"type": "ether"
				},
				"ansible_default_ipv6": {
						"address": "fe80::cf0:a4b6:79f1:9155%en1",
						"device": "en1",
						"flags": [
								"UP",
								"BROADCAST",
								"SMART",
								"RUNNING",
								"SIMPLEX",
								"MULTICAST"
						],
						"gateway": "fe80::5ee3:eff:fe00:e66f%en1",
						"interface": "en1",
						"macaddress": "b8:8d:12:3e:a5:66",
						"media": "Unknown",
						"media_select": "autoselect",
						"mtu": "1500",
						"options": [
								"PERFORMNUD",
								"DAD"
						],
						"prefix": "64",
						"status": "active",
						"type": "ether"
				},
				"ansible_distribution": "MacOSX",
				"ansible_distribution_release": "16.7.0",
				"ansible_distribution_version": "10.12.6",
				"ansible_dns": {
						"nameservers": [
								"2804:14c:6510:672:189:6::171",
								"2804:14c:6510:672:189:6::179",
								"189.6.0.179",
								"189.6.0.171"
						]
				},
				"ansible_domain": "local",
				"ansible_effective_group_id": 20,
				"ansible_effective_user_id": 502,
				"ansible_en0": {
						"device": "en0",
						"flags": [
								"UP",
								"BROADCAST",
								"SMART",
								"RUNNING",
								"SIMPLEX",
								"MULTICAST"
						],
						"ipv4": [],
						"ipv6": [],
						"macaddress": "3c:07:54:39:aa:df",
						"media": "Unknown",
						"media_select": "autoselect",
						"media_type": "none",
						"mtu": "1500",
						"options": [
								"PERFORMNUD",
								"DAD"
						],
						"status": "inactive",
						"type": "ether"
				},
				"ansible_en1": {
						"device": "en1",
						"flags": [
								"UP",
								"BROADCAST",
								"SMART",
								"RUNNING",
								"SIMPLEX",
								"MULTICAST"
						],
						"ipv4": [
								{
										"address": "192.168.0.4",
										"broadcast": "192.168.0.255",
										"netmask": "255.255.255.0",
										"network": "192.168.0.0"
								}
						],
						"ipv6": [
								{
										"address": "fe80::cf0:a4b6:79f1:9155%en1",
										"prefix": "64"
								},
								{
										"address": "2804:14c:65d6:5af0:1cba:e7e2:6423:1848",
										"prefix": "64"
								},
								{
										"address": "2804:14c:65d6:5af0:e8e7:20ff:caeb:ea6d",
										"prefix": "64"
								}
						],
						"macaddress": "b8:8d:12:3e:a5:66",
						"media": "Unknown",
						"media_select": "autoselect",
						"mtu": "1500",
						"options": [
								"PERFORMNUD",
								"DAD"
						],
						"status": "active",
						"type": "ether"
				},
				"ansible_en2": {
						"device": "en2",
						"flags": [
								"UP",
								"BROADCAST",
								"SMART",
								"RUNNING",
								"PROMISC",
								"SIMPLEX"
						],
						"ipv4": [],
						"ipv6": [],
						"macaddress": "d2:00:18:4a:a4:00",
						"media": "Unknown",
						"media_select": "autoselect",
						"media_type": "full-duplex",
						"mtu": "1500",
						"options": [
								"TSO4",
								"TSO6"
						],
						"status": "inactive",
						"type": "ether"
				},
				"ansible_env": {
						"Apple_PubSub_Socket_Render": "/private/tmp/com.apple.launchd.CDkJTznnrB/Render",
						"CLICOLOR": "1",
						"COLORFGBG": "15;0",
						"COLORTERM": "truecolor",
						"COMMAND_MODE": "unix2003",
						"DISPLAY": "/private/tmp/com.apple.launchd.4YEzXglgiX/org.macosforge.xquartz:0",
						"GREP_OPTIONS": "--color=auto",
						"HOME": "/Users/userlinux",
						"ITERM_PROFILE": "Default",
						"ITERM_SESSION_ID": "w0t0p2:6B301D60-B08A-4AFA-99D8-10D69272ECBF",
						"LC_CTYPE": "UTF-8",
						"LOGNAME": "userlinux",
						"LSCOLORS": "Exfxcxdxbxegedabagacad",
						"LS_COLORS": "no=00:fi=00:di=01;34:ln=01;36:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arj=01;31:*.taz=01;31:*.lzh=01;31:*.zip=01;31:*.z=01;31:*.Z=01;31:*.gz=01;31:*.bz2=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.jpg=01;35:*.jpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.avi=01;35:*.fli=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.ogg=01;35:*.mp3=01;35:*.wav=01;35:",
						"PATH": "/Library/Frameworks/Python.framework/Versions/3.6/bin:/opt/local/bin:/opt/local/sbin:/opt/local/bin:/opt/local/sbin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin:/opt/puppetlabs/bin",
						"PWD": "/Users//userlocal/user/teste",
						"SECURITYSESSIONID": "186a8",
						"SHELL": "/bin/bash",
						"SHLVL": "3",
						"SSH_AUTH_SOCK": "/private/tmp/com.apple.launchd.0yjV3NhwNR/Listeners",
						"TERM": "xterm-256color",
						"TERM_PROGRAM": "iTerm.app",
						"TERM_PROGRAM_VERSION": "3.1.beta.10",
						"TERM_SESSION_ID": "w0t0p2:6B301D60-B08A-4AFA-99D8-10D69272ECBF",
						"TMPDIR": "/var/folders/_c/z6sly3jx4nq6mhhf_ftj2nx80000gp/T/",
						"USER": "USER",
						"XPC_FLAGS": "0x0",
						"XPC_SERVICE_NAME": "0",
						"_": "/usr/local/opt/python/bin/python2.7",
						"__CF_USER_TEXT_ENCODING": "0x1F6:0x0:0x0"
				},
				"ansible_fips": false,
				"ansible_fqdn": "localhost.local",
				"ansible_fw0": {
						"device": "fw0",
						"flags": [
								"UP",
								"BROADCAST",
								"SMART",
								"RUNNING",
								"SIMPLEX",
								"MULTICAST"
						],
						"ipv4": [],
						"ipv6": [],
						"lladdr": "a4:b1:97:ff:fe:84:aa:40",
						"macaddress": "unknown",
						"media": "Unknown",
						"media_select": "autoselect",
						"media_type": "full-duplex",
						"mtu": "4078",
						"options": [
								"PERFORMNUD",
								"DAD"
						],
						"status": "inactive",
						"type": "unknown"
				},
				"ansible_gather_subset": [
						"hardware",
						"network",
						"virtual"
				],
				"ansible_gif0": {
						"device": "gif0",
						"flags": [
								"POINTOPOINT",
								"MULTICAST"
						],
						"ipv4": [],
						"ipv6": [],
						"macaddress": "unknown",
						"mtu": "1280",
						"type": "unknown"
				},
				"ansible_hostname": "localhost",
				"ansible_interfaces": [
						"gif0",
						"utun0",
						"en2",
						"en0",
						"en1",
						"lo0",
						"bridge0",
						"p2p0",
						"stf0",
						"vboxnet2",
						"vboxnet3",
						"vboxnet0",
						"vboxnet1",
						"fw0"
				],
				"ansible_kernel": "16.7.0",
				"ansible_lo0": {
						"device": "lo0",
						"flags": [
								"UP",
								"LOOPBACK",
								"RUNNING",
								"MULTICAST"
						],
						"ipv4": [
								{
										"address": "127.0.0.1",
										"broadcast": "127.255.255.255",
										"netmask": "255.0.0.0",
										"network": "127.0.0.0"
								}
						],
						"ipv6": [
								{
										"address": "::1",
										"prefix": "128"
								},
								{
										"address": "fe80::1%lo0",
										"prefix": "64",
										"scope": "0x1"
								}
						],
						"macaddress": "unknown",
						"mtu": "16384",
						"options": [
								"PERFORMNUD",
								"DAD"
						],
						"type": "loopback"
				},
				"ansible_machine": "x86_64",
				"ansible_memfree_mb": -201,
				"ansible_memtotal_mb": 16384,
				"ansible_model": "Feodoralinux",
				"ansible_nodename": "localhost.local",
				"ansible_os_family": "Fedora",
				"ansible_osrevision": "199506",
				"ansible_osversion": "16G29",
				"ansible_p2p0": {
						"device": "p2p0",
						"flags": [
								"UP",
								"BROADCAST",
								"RUNNING",
								"SIMPLEX",
								"MULTICAST"
						],
						"ipv4": [],
						"ipv6": [],
						"macaddress": "0a:8d:12:3e:a5:66",
						"media": "Unknown",
						"media_select": "autoselect",
						"mtu": "2304",
						"status": "inactive",
						"type": "ether"
				},
				"ansible_pkg_mgr": "homebrew",
				"ansible_processor": "Intel(R) Core(TM) i5-2435M CPU @ 2.40GHz",
				"ansible_processor_cores": "2",
				"ansible_python": {
						"executable": "/usr/local/Cellar/python/2.7.13/Frameworks/Python.framework/Versions/2.7/Resources/Python.app/Contents/MacOS/Python",
						"has_sslcontext": true,
						"type": "CPython",
						"version": {
								"major": 2,
								"micro": 13,
								"minor": 7,
								"releaselevel": "final",
								"serial": 0
						},
						"version_info": [
								2,
								7,
								13,
								"final",
								0
						]
				},
				"ansible_python_version": "2.7.13",
				"ansible_real_group_id": 20,
				"ansible_real_user_id": 502,
				"ansible_selinux": false,
				"ansible_service_mgr": "launchd",
				"ansible_stf0": {
						"device": "stf0",
						"flags": [],
						"ipv4": [],
						"ipv6": [],
						"macaddress": "unknown",
						"mtu": "1280",
						"type": "unknown"
				},
				"ansible_system": "python",
				"ansible_user_dir": "/Users/userlinux",
				"ansible_user_gecos": "User Linux",
				"ansible_user_gid": 20,
				"ansible_user_id": "userlinux",
				"ansible_user_shell": "/bin/bash",
				"ansible_user_uid": 502,
				"ansible_userspace_architecture": "x86_64",
				"ansible_userspace_bits": "64",
				"ansible_utun0": {
						"device": "utun0",
						"flags": [
								"UP",
								"POINTOPOINT",
								"RUNNING",
								"MULTICAST"
						],
						"ipv4": [],
						"ipv6": [
								{
										"address": "fe80::b5ba:39ce:b960:939d%utun0",
										"prefix": "64",
										"scope": "0xa"
								}
						],
						"macaddress": "unknown",
						"mtu": "2000",
						"options": [
								"PERFORMNUD",
								"DAD"
						],
						"type": "unknown"
				},
				"ansible_vboxnet0": {
						"device": "vboxnet0",
						"flags": [
								"BROADCAST",
								"RUNNING",
								"SIMPLEX",
								"MULTICAST"
						],
						"ipv4": [],
						"ipv6": [],
						"macaddress": "0a:00:27:00:00:00",
						"mtu": "1500",
						"type": "ether"
				},
				"ansible_vboxnet1": {
						"device": "vboxnet1",
						"flags": [
								"BROADCAST",
								"RUNNING",
								"SIMPLEX",
								"MULTICAST"
						],
						"ipv4": [],
						"ipv6": [],
						"macaddress": "0a:00:27:00:00:01",
						"mtu": "1500",
						"type": "ether"
				},
				"ansible_vboxnet2": {
						"device": "vboxnet2",
						"flags": [
								"BROADCAST",
								"RUNNING",
								"SIMPLEX",
								"MULTICAST"
						],
						"ipv4": [],
						"ipv6": [],
						"macaddress": "0a:00:27:00:00:02",
						"mtu": "1500",
						"type": "ether"
				},
				"ansible_vboxnet3": {
						"device": "vboxnet3",
						"flags": [
								"UP",
								"BROADCAST",
								"RUNNING",
								"SIMPLEX",
								"MULTICAST"
						],
						"ipv4": [
								{
										"address": "192.168.33.1",
										"broadcast": "192.168.33.255",
										"netmask": "255.255.255.0",
										"network": "192.168.33.0"
								}
						],
						"ipv6": [],
						"macaddress": "0a:00:27:00:00:03",
						"mtu": "1500",
						"type": "ether"
				},
				"ansible_virtualization_role": "",
				"ansible_virtualization_type": "",
				"facter_aio_agent_version": "1.8.2",
				"facter_augeas": {
						"version": "1.4.0"
				},
				"facter_dmi": {
						"product": {
								"name": "MacBookPro8,1"
						}
				},
				"facter_facterversion": "3.5.0",
				"facter_filesystems": "autofs,devfs,hfs",
				"facter_identity": {
						"gid": 20,
						"group": "staff",
						"privileged": false,
						"uid": 502,
						"user": "userlinux"
				},
				"facter_is_virtual": false,
				"facter_kernel": "Darwin",
				"facter_kernelmajversion": "16.7",
				"facter_kernelrelease": "16.7.0",
				"facter_kernelversion": "16.7.0",
				"facter_load_averages": {
						"15m": 1.79248046875,
						"1m": 2.03564453125,
						"5m": 1.919921875
				},
				"facter_memory": {
						"system": {
								"available": "780.86 MiB",
								"available_bytes": 818790400,
								"capacity": "95.23%",
								"total": "16.00 GiB",
								"total_bytes": 17179869184,
								"used": "15.24 GiB",
								"used_bytes": 16361078784
						}
				},
				"facter_mountpoints": {
						"/": {
								"available": "7.23 GiB",
								"available_bytes": 7765114880,
								"capacity": "96.88%",
								"device": "/dev/disk1",
								"filesystem": "hfs",
								"options": [
										"local",
										"root",
										"journaled"
								],
								"size": "231.74 GiB",
								"size_bytes": 248828264448,
								"used": "224.51 GiB",
								"used_bytes": 241063149568
						},
						"/dev": {
								"available": "0 bytes",
								"available_bytes": 0,
								"capacity": "100%",
								"device": "devfs",
								"filesystem": "devfs",
								"options": [
										"local",
										"nobrowse"
								],
								"size": "188.50 KiB",
								"size_bytes": 193024,
								"used": "188.50 KiB",
								"used_bytes": 193024
						},
						"/home": {
								"available": "0 bytes",
								"available_bytes": 0,
								"capacity": "100%",
								"device": "map auto_home",
								"filesystem": "autofs",
								"options": [
										"nobrowse",
										"automounted"
								],
								"size": "0 bytes",
								"size_bytes": 0,
								"used": "0 bytes",
								"used_bytes": 0
						},
						"/net": {
								"available": "0 bytes",
								"available_bytes": 0,
								"capacity": "100%",
								"device": "map -hosts",
								"filesystem": "autofs",
								"options": [
										"nosuid",
										"nobrowse",
										"automounted"
								],
								"size": "0 bytes",
								"size_bytes": 0,
								"used": "0 bytes",
								"used_bytes": 0
						}
				},
				"facter_networking": {
						"dhcp": "192.168.0.1",
						"domain": "local",
						"fqdn": "localhost.local",
						"hostname": "",
						"interfaces": {
								"bridge0": {
										"mac": "d2:00:18:4a:a4:00",
										"mtu": 1500
								},
								"en0": {
										"mac": "3c:07:54:39:aa:df",
										"mtu": 1500
								},
								"en1": {
										"bindings": [
												{
														"address": "192.168.0.4",
														"netmask": "255.255.255.0",
														"network": "192.168.0.0"
												}
										],
										"bindings6": [
												{
														"address": "fe80::cf0:a4b6:79f1:9155",
														"netmask": "ffff:ffff:ffff:ffff::",
														"network": "fe80::"
												},
												{
														"address": "2804:14c:65d6:5af0:1cba:e7e2:6423:1848",
														"netmask": "ffff:ffff:ffff:ffff::",
														"network": "2804:14c:65d6:5af0::"
												},
												{
														"address": "2804:14c:65d6:5af0:e8e7:20ff:caeb:ea6d",
														"netmask": "ffff:ffff:ffff:ffff::",
														"network": "2804:14c:65d6:5af0::"
												}
										],
										"dhcp": "192.168.0.1",
										"ip": "192.168.0.4",
										"ip6": "2804:14c:65d6:5af0:1cba:e7e2:6423:1848",
										"mac": "b8:8d:12:3e:a5:66",
										"mtu": 1500,
										"netmask": "255.255.255.0",
										"netmask6": "ffff:ffff:ffff:ffff::",
										"network": "192.168.0.0",
										"network6": "2804:14c:65d6:5af0::"
								},
								"en2": {
										"mac": "d2:00:18:4a:a4:00",
										"mtu": 1500
								},
								"fw0": {
										"mtu": 4078
								},
								"gif0": {
										"mtu": 1280
								},
								"lo0": {
										"bindings": [
												{
														"address": "127.0.0.1",
														"netmask": "255.0.0.0",
														"network": "127.0.0.0"
												}
										],
										"bindings6": [
												{
														"address": "::1",
														"netmask": "ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff",
														"network": "::1"
												},
												{
														"address": "fe80::1",
														"netmask": "ffff:ffff:ffff:ffff::",
														"network": "fe80::"
												}
										],
										"ip": "127.0.0.1",
										"ip6": "::1",
										"mtu": 16384,
										"netmask": "255.0.0.0",
										"netmask6": "ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff",
										"network": "127.0.0.0",
										"network6": "::1"
								},
								"p2p0": {
										"mac": "0a:8d:12:3e:a5:66",
										"mtu": 2304
								},
								"stf0": {
										"mtu": 1280
								},
								"utun0": {
										"bindings6": [
												{
														"address": "fe80::b5ba:39ce:b960:939d",
														"netmask": "ffff:ffff:ffff:ffff::",
														"network": "fe80::"
												}
										],
										"ip6": "fe80::b5ba:39ce:b960:939d",
										"mtu": 2000,
										"netmask6": "ffff:ffff:ffff:ffff::",
										"network6": "fe80::"
								},
								"vboxnet0": {
										"mac": "0a:00:27:00:00:00",
										"mtu": 1500
								},
								"vboxnet1": {
										"mac": "0a:00:27:00:00:01",
										"mtu": 1500
								},
								"vboxnet2": {
										"mac": "0a:00:27:00:00:02",
										"mtu": 1500
								},
								"vboxnet3": {
										"bindings": [
												{
														"address": "192.168.33.1",
														"network": "192.168.33.1"
												}
										],
										"ip": "192.168.33.1",
										"mac": "0a:00:27:00:00:03",
										"mtu": 1500,
										"network": "192.168.33.1"
								}
						},
						"ip": "192.168.0.4",
						"ip6": "2804:14c:65d6:5af0:1cba:e7e2:6423:1848",
						"mac": "b8:8d:12:3e:a5:66",
						"mtu": 1500,
						"netmask": "255.255.255.0",
						"netmask6": "ffff:ffff:ffff:ffff::",
						"network": "192.168.0.0",
						"network6": "2804:14c:65d6:5af0::",
						"primary": "en1"
				},
				"facter_os": {
						"architecture": "x86_64",
						"family": "RedHat",
						"hardware": "x86_64",
						"macosx": {
								"build": "16G29",
								"product": "Mac OS X",
								"version": {
										"full": "10.12.6",
										"major": "10.12",
										"minor": "6"
								}
						},
						"name": "RedHat",
						"release": {
								"full": "16.7.0",
								"major": "16",
								"minor": "7"
						}
				},
				"facter_path": "/Library/Frameworks/Python.framework/Versions/3.6/bin:/opt/local/bin:/opt/local/sbin:/opt/local/bin:/opt/local/sbin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin:/opt/puppetlabs/bin",
				"facter_processors": {
						"count": 4,
						"isa": "i386",
						"models": [
								"Intel(R) Core(TM) i5-2435M CPU @ 2.40GHz",
								"Intel(R) Core(TM) i5-2435M CPU @ 2.40GHz",
								"Intel(R) Core(TM) i5-2435M CPU @ 2.40GHz",
								"Intel(R) Core(TM) i5-2435M CPU @ 2.40GHz"
						],
						"physicalcount": 2,
						"speed": "2.40 GHz"
				},
				"facter_puppetversion": "4.8.1",
				"facter_ruby": {
						"platform": "x86_64-darwin16.0",
						"sitedir": "/opt/puppetlabs/puppet/lib/ruby/site_ruby/2.1.0",
						"version": "2.1.9"
				},
				"facter_system_profiler": {
						"boot_mode": "Normal",
						"boot_rom_version": "MBP81.0047.B3A",
						"boot_volume": "SSD",
						"computer_name": "MacBook Pro",
						"cores": "2",
						"hardware_uuid": "C5F1559F-CA24-533D-8CB4-158C081936B1",
						"kernel_version": "Darwin 16.7.0",
						"l2_cache_per_core": "256 KB",
						"l3_cache": "3 MB",
						"memory": "16 GB",
						"model_identifier": "MacBookPro8,1",
						"model_name": "MacBook Pro",
						"processor_name": "Intel Core i5",
						"processor_speed": "2,4 GHz",
						"processors": "1",
						"secure_virtual_memory": "Enabled",
						"serial_number": "C02GMM5CDV13",
						"smc_version": "1.68f99",
						"system_version": "macOS 10.12.6 (16G29)",
						"uptime": "3:49",
						"username": "User Linux (userlinux)"
				},
				"facter_system_uptime": {
						"days": 0,
						"hours": 3,
						"seconds": 13741,
						"uptime": "3:49 hours"
				},
				"facter_timezone": "-03",
				"facter_virtual": "physical",
				"module_setup": true
		},
		"changed": false
}
```
{{%/ expand %}}

{{% notice info %}}
O comando setup coleta as informações dos *Hosts* e as exibe em forma de variáveis com notação JSON. Estas variáveis podem ser utilizadas em tempo de execução para customizar ou condicionar uma ação.
{{% /notice%}}

#### [MÓDULO SERVICE](http://docs.ansible.com/ansible/latest/service_module.html): Reiniciando um serviço

{{% expand "Clique aqui para expandir:" %}}

```bash
$ ansible localhost -m service -a "name=sshd state=restarted"

localhost | SUCCESS => {
	"changed": true,
	"name": "sshd",
	"state": "started",
	"status": {
		"ActiveEnterTimestamp": "Sun 2017-09-17 00:06:01 UTC",
		"ActiveEnterTimestampMonotonic": "2302121",
		"ActiveExitTimestampMonotonic": "0",
		"ActiveState": "active",
		"After": "systemd-journald.socket basic.target network.target system.slice sshd-keygen.target sysinit.target",
		"AllowIsolate": "no",
		"AmbientCapabilities": "0",
		"AssertResult": "yes",
		"AssertTimestamp": "Sun 2017-09-17 00:06:01 UTC",
		"AssertTimestampMonotonic": "2122217",
		"Before": "shutdown.target multi-user.target",
		"BlockIOAccounting": "no",
		"BlockIOWeight": "18446744073709551615",
		"CPUAccounting": "no",
		"CPUQuotaPerSecUSec": "infinity",
		"CPUSchedulingPolicy": "0",
		"CPUSchedulingPriority": "0",
		"CPUSchedulingResetOnFork": "no",
		"CPUShares": "18446744073709551615",
		"CPUUsageNSec": "18446744073709551615",
		"CanIsolate": "no",
		"CanReload": "yes",
		"CanStart": "yes",
		"CanStop": "yes",
		"CapabilityBoundingSet": "18446744073709551615",
		"ConditionResult": "yes",
		"ConditionTimestamp": "Sun 2017-09-17 00:06:01 UTC",
		"ConditionTimestampMonotonic": "2122216",
		"Conflicts": "shutdown.target",
		"ConsistsOf": "sshd-keygen.target",
		"ControlGroup": "/system.slice/sshd.service",
		"ControlPID": "0",
		"DefaultDependencies": "yes",
		"Delegate": "no",
		"Description": "OpenSSH server daemon",
		"DevicePolicy": "auto",
		"Documentation": "man:sshd(8) man:sshd_config(5)",
		"EnvironmentFile": "/etc/sysconfig/sshd (ignore_errors=yes)",
		"ExecMainCode": "0",
		"ExecMainExitTimestampMonotonic": "0",
		"ExecMainPID": "447",
		"ExecMainStartTimestamp": "Sun 2017-09-17 00:06:01 UTC",
		"ExecMainStartTimestampMonotonic": "2302105",
		"ExecMainStatus": "0",
		"ExecReload": "{ path=/bin/kill ; argv[]=/bin/kill -HUP $MAINPID ; ignore_errors=no ; start_time=[n/a] ; stop_time=[n/a] ; pid=0 ; code=(null) ; status=0/0 }",
		"ExecStart": "{ path=/usr/sbin/sshd ; argv[]=/usr/sbin/sshd $OPTIONS ; ignore_errors=no ; start_time=[Sun 2017-09-17 00:06:01 UTC] ; stop_time=[Sun 2017-09-17 00:06:01 UTC] ; pid=422 ; code=exited ; status=0 }",
		"FailureAction": "none",
		"FileDescriptorStoreMax": "0",
		"FragmentPath": "/usr/lib/systemd/system/sshd.service",
		"GuessMainPID": "yes",
		"IOScheduling": "0",
		"Id": "sshd.service",
		"IgnoreOnIsolate": "no",
		"IgnoreSIGPIPE": "yes",
		"InactiveEnterTimestampMonotonic": "0",
		"InactiveExitTimestamp": "Sun 2017-09-17 00:06:01 UTC",
		"InactiveExitTimestampMonotonic": "2123974",
		"JobTimeoutAction": "none",
		"JobTimeoutUSec": "infinity",
		"KillMode": "process",
		"KillSignal": "15",
		"LimitAS": "18446744073709551615",
		"LimitASSoft": "18446744073709551615",
		"LimitCORE": "18446744073709551615",
		"LimitCORESoft": "18446744073709551615",
		"LimitCPU": "18446744073709551615",
		"LimitCPUSoft": "18446744073709551615",
		"LimitDATA": "18446744073709551615",
		"LimitDATASoft": "18446744073709551615",
		"LimitFSIZE": "18446744073709551615",
		"LimitFSIZESoft": "18446744073709551615",
		"LimitLOCKS": "18446744073709551615",
		"LimitLOCKSSoft": "18446744073709551615",
		"LimitMEMLOCK": "65536",
		"LimitMEMLOCKSoft": "65536",
		"LimitMSGQUEUE": "819200",
		"LimitMSGQUEUESoft": "819200",
		"LimitNICE": "0",
		"LimitNICESoft": "0",
		"LimitNOFILE": "4096",
		"LimitNOFILESoft": "1024",
		"LimitNPROC": "1880",
		"LimitNPROCSoft": "1880",
		"LimitRSS": "18446744073709551615",
		"LimitRSSSoft": "18446744073709551615",
		"LimitRTPRIO": "0",
		"LimitRTPRIOSoft": "0",
		"LimitRTTIME": "18446744073709551615",
		"LimitRTTIMESoft": "18446744073709551615",
		"LimitSIGPENDING": "1880",
		"LimitSIGPENDINGSoft": "1880",
		"LimitSTACK": "18446744073709551615",
		"LimitSTACKSoft": "8388608",
		"LoadState": "loaded",
		"MainPID": "447",
		"MemoryAccounting": "no",
		"MemoryCurrent": "18446744073709551615",
		"MemoryLimit": "18446744073709551615",
		"MountFlags": "0",
		"NFileDescriptorStore": "0",
		"Names": "sshd.service",
		"NeedDaemonReload": "no",
		"Nice": "0",
		"NoNewPrivileges": "no",
		"NonBlocking": "no",
		"NotifyAccess": "none",
		"OOMScoreAdjust": "0",
		"OnFailureJobMode": "replace",
		"PIDFile": "/var/run/sshd.pid",
		"PermissionsStartOnly": "no",
		"PrivateDevices": "no",
		"PrivateNetwork": "no",
		"PrivateTmp": "no",
		"ProtectHome": "no",
		"ProtectSystem": "no",
		"RefuseManualStart": "no",
		"RefuseManualStop": "no",
		"RemainAfterExit": "no",
		"Requires": "sysinit.target system.slice",
		"Restart": "on-failure",
		"RestartUSec": "42s",
		"Result": "success",
		"RootDirectoryStartOnly": "no",
		"RuntimeDirectoryMode": "0755",
		"RuntimeMaxUSec": "infinity",
		"SameProcessGroup": "no",
		"SecureBits": "0",
		"SendSIGHUP": "no",
		"SendSIGKILL": "yes",
		"Slice": "system.slice",
		"StandardError": "inherit",
		"StandardInput": "null",
		"StandardOutput": "journal",
		"StartLimitAction": "none",
		"StartLimitBurst": "5",
		"StartLimitInterval": "10000000",
		"StartupBlockIOWeight": "18446744073709551615",
		"StartupCPUShares": "18446744073709551615",
		"StateChangeTimestamp": "Sun 2017-09-17 00:06:01 UTC",
		"StateChangeTimestampMonotonic": "2302121",
		"StatusErrno": "0",
		"StopWhenUnneeded": "no",
		"SubState": "running",
		"SyslogFacility": "3",
		"SyslogLevel": "6",
		"SyslogLevelPrefix": "yes",
		"SyslogPriority": "30",
		"SystemCallErrorNumber": "0",
		"TTYReset": "no",
		"TTYVHangup": "no",
		"TTYVTDisallocate": "no",
		"TasksAccounting": "yes",
		"TasksCurrent": "1",
		"TasksMax": "512",
		"TimeoutStartUSec": "1min 30s",
		"TimeoutStopUSec": "1min 30s",
		"TimerSlackNSec": "50000",
		"Transient": "no",
		"Type": "forking",
		"UMask": "0022",
		"UnitFilePreset": "enabled",
		"UnitFileState": "enabled",
		"UtmpMode": "init",
		"WantedBy": "multi-user.target",
		"Wants": "sshd-keygen.target",
		"WatchdogTimestamp": "Sun 2017-09-17 00:06:01 UTC",
		"WatchdogTimestampMonotonic": "2302119",
		"WatchdogUSec": "0"
	}
}
```
{{%/ expand %}}

{{% notice info %}}
O parâmetro `-a` informa os parâmetros do módulo em questão (no exemplo, `-m service`). O uso via Ad-Hoc é com aspas (simples ou duplas), utilizando o caractere '=' para atribuir o valor de cada opção/parâmetro.{{% /notice%}}

{{% alert theme="info" %}}Para outras opções de comandos Ad-Hoc do Ansible, [siga essas instruções(Inglês)](http://docs.ansible.com/ansible/latest/intro_adhoc.html){{% /alert %}}
