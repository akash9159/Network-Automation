# Network-Automation
Code of Python Automation
from netmiko import ConnectHandler

switch_ips = [
    "1.1.1.2",
    "1.1.1.3",
    "1.1.1.4",
    "1.1.1.5",
    "1.1.1.6",
    "1.1.1.7",
]

username = "admin"
password = ""

for ip in switch_ips:
    print(f"\nConnecting to {ip}...")

    device = {
        "device_type": "extreme_exos",
        "host": ip,
        "username": username,
        "password": password,
    }

    try:
        connection = ConnectHandler(**device)

        commands = [
            "create vlan nms tag 250",
            "create vlan camera tag 1130",
            "configure camera add ports 47-54 tagged",
            "configure nms add ports 47-54 tagged",
            "save"
        ]
        
        output = connection.send_config_set(commands)
        print(f"Configuration pushed to {ip}")
        print(output)

        connection.disconnect()

    except Exception as e:
        print(f"Failed on {ip}: {e}")
