# phyton3-mac-avtomatizacia
import subprocess
import optparse
import re

def get_arguments():
    parser = optparse.OptionParser()
    parser.add_option("-i", "--interface", dest="interface", help="Interface to change MAQ address")
    parser.add_option("-e", "--eth0", dest="new_eth0", help="new MAC Address")
    (options, arguments) = parser.parse_args()
    if not options.interface:
        parser.error("[-] please specify an interface, use --help for more info")
    elif not options.new_eth0:
        parser.error("[-] Please specify a new mac, use --help for more info.")
    return options
def change_mac(interface, new_eth0):
    print("[+] Changing MAC address for " + interface + " to " + new_eth0)
    subprocess.call(["ifconfig", interface, "down"])
    subprocess.call(["ifconfig", interface, "hw", "ether", new_eth0])
    subprocess.call(["ifconfig", interface, "up"])

def get_current_mac(interface):
    ifconfig_result = subprocess.check_output(["ifconfig", interface])
    mac_adrdress_search_result = re.search(r"\w\w:\w\w:\w\w:\w\w:\w\w:\w\w", ifconfig_result)
    if mac_adrdress_search_result:
        return mac_adrdress_search_result.group(0)
    else:
        print("[-] Could not read MAC addres")

options = get_arguments()
curent_mac = get_current_mac(options.interface)
print("Curent MAC  address is " + str(curent_mac))
change_mac(options.interface, options.new_eth0)
curent_mac = get_current_mac(options.interface)
if curent_mac == options.new_eth0:
    print("[+] ETH0  misamarti sheicvala " + curent_mac + " misamartit ")
else:
    print("[-] ETH misamarti ar shecvlila")
