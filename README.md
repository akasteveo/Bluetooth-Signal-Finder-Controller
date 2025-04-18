# Bluetooth-Signal-Finder-Controller
The Bluetooth Signal Finder &amp; Controller App transforms your Flipper Zero into a powerful tool for interacting with nearby Bluetooth devices. It provides the ability to scan for nearby Bluetooth signals, connect to a device, and control its media playback and volume seamlessly.
import os

class BluetoothSignalFinderAndController:
    PAIRING_FILE = "paired_devices.txt"

    def __init__(self):
        self.devices = [
            {"name": "Device 1", "address": "00:11:22:33:44:55", "rssi": -40},
            {"name": "Device 2", "address": "66:77:88:99:AA:BB", "rssi": -60},
            {"name": "Device 3", "address": "CC:DD:EE:FF:00:11", "rssi": -50},
        ]
        self.paired_devices = self.load_paired_devices()  # List to store paired device MAC addresses
        self.device_count = len(self.devices)
        self.selected_device = 0
        self.is_connected = False

    def scan_bluetooth_devices(self):
        print("Scanning for devices...")
        if not self.devices:
            print("No devices found.")
            return False

        for i, device in enumerate(self.devices):
            print(f"{i + 1}. {device['name']} ({device['address']}) RSSI: {device['rssi']} dBm")
        print("Scan complete.")
        return True

    def pair_device(self, device_index):
        if 0 <= device_index < self.device_count:
            device = self.devices[device_index]
            if device["address"] not in self.paired_devices:
                self.paired_devices.append(device["address"])
                self.save_paired_devices()
                print(f"Paired with {device['name']} ({device['address']}).")
            else:
                print(f"{device['name']} is already paired.")
        else:
            print("Invalid device selection.")

    def unpair_device(self, device_index):
        if 0 <= device_index < self.device_count:
            device = self.devices[device_index]
            if device["address"] in self.paired_devices:
                self.paired_devices.remove(device["address"])
                self.save_paired_devices()
                print(f"Unpaired {device['name']} ({device['address']}).")
            else:
                print(f"{device['name']} is not paired.")
        else:
            print("Invalid device selection.")

    def connect_to_device(self, device_index):
        if 0 <= device_index < self.device_count:
            device = self.devices[device_index]
            print(f"Attempting to connect to {device['name']}...")
            # Simulate connection success
            connection_successful = True
            if connection_successful:
                self.is_connected = True
                print(f"Connected to {device['name']} ({device['address']}).")
            else:
                print(f"Failed to connect to {device['name']}. Please retry.")
        else:
            print("Invalid device selection.")

    def auto_reconnect(self):
        print("Attempting auto-reconnect...")
        for device in self.devices:
            if device["address"] in self.paired_devices:
                print(f"Auto-reconnecting to {device['name']}...")
                self.connect_to_device(self.devices.index(device))
                return
        print("No paired devices available for reconnection.")

    def disconnect_device(self):
        if self.is_connected:
            device = self.devices[self.selected_device]
            print(f"Disconnected from {device['name']} ({device['address']}).")
            self.is_connected = False
        else:
            print("No device to disconnect.")

    def send_media_command(self, command):
        if self.is_connected:
            print(f"Sending command: {command}")
        else:
            print("No device connected. Cannot send command.")

    def save_paired_devices(self):
        with open(self.PAIRING_FILE, "w") as f:
            for address in self.paired_devices:
                f.write(address + "\n")
        print("Paired devices saved.")

    def load_paired_devices(self):
        if os.path.exists(self.PAIRING_FILE):
            with open(self.PAIRING_FILE, "r") as f:
                return [line.strip() for line in f.readlines()]
        return []

    def run(self):
        print("Welcome to Bluetooth Signal Finder & Controller!")
        while True:
            print("\nMain Menu:")
            print("1. Scan for Devices")
            print("2. Pair a Device")
            print("3. Unpair a Device")
            print("4. Auto-Reconnect")
            print("5. Send Media Command (Play/Pause)")
            print("6. Disconnect Device")
            print("7. Exit")

            choice = input("Choose an option: ")

            if choice == "1":
                self.scan_bluetooth_devices()
            elif choice == "2":
                device_index = int(input("Enter device number to pair: ")) - 1
                self.pair_device(device_index)
            elif choice == "3":
                device_index = int(input("Enter device number to unpair: ")) - 1
                self.unpair_device(device_index)
            elif choice == "4":
                self.auto_reconnect()
            elif choice == "5":
                self.send_media_command("Play/Pause")
            elif choice == "6":
                self.disconnect_device()
            elif choice == "7":
                print("Exiting. Goodbye!")
                break
            else:
                print("Invalid choice. Please try again.")


# Run the application
app = BluetoothSignalFinderAndController()



