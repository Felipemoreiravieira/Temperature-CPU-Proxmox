# Temperature-CPU-Proxmox
Server CPU temperature Proxmox

insert the codes in the Proxmox PVE terminal in order (follow the PDF document)
==============================================================
apt-get install lm-sensors
==============================================================
sensors
==============================================================
vim /usr/share/per15/PVE/API2/Nodes.pm
==============================================================
$res->{thermalstate} = `sensors -j`;
==============================================================
vim /usr/share/pve-manager/js/pvemanagerlib.js
==============================================================
itemId:	'thermal',
colspan: 2,
printBar: false,
title: gettext('CPU Thermal State'),
textField: 'thermalstate',
renderer:function(value){'
	let objValue = JSON.parse(value);
	let cores = objValue["coretemp-isa-0000"]
	let items = object.keys(cores).filter(item => /Core/.test(item));
	let str = '';
	items.forEach((x, idx) => {
			str += cores[x]['temp${idx+2}_input'] + ' ';
	});
	str += 'ºc';
	return str;
}
==============================================================
systemctl restart pveproxy
