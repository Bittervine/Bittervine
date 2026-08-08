# Lubuntu on the Lenovo MIIX 3-1030

This folder contains the public notes, tested DKMS package, and source patch from a real Lenovo MIIX 3-1030 Wi-Fi investigation.

The RTL8723BS could scan access points but host-to-card CMD53 writes failed with SDIO CRC errors. Merely lowering the clock did not help because the bus remained in high-speed timing. Clearing `SDIO_SPEED_EHS` before the first Realtek I/O and returning both host and card to legacy 25 MHz timing fixed the connection.

- Read the full guide: [`index.html`](index.html)
- Recommended tested package: [`downloads/r8723bs-miix3-dkms-1.1.tar.gz`](downloads/r8723bs-miix3-dkms-1.1.tar.gz)
- Core kernel patch: [`patches/r8723bs-miix3-legacy-sdio.patch`](patches/r8723bs-miix3-legacy-sdio.patch)
- Modprobe configuration: [`r8723bs-miix3.conf`](r8723bs-miix3.conf)

The DKMS archive contains the exact source that passed a clean-reboot test on Ubuntu kernel `7.0.0-28-generic`: a concurrent 64 MiB download and 5,000 full-sized pings completed with 0% loss, zero RX/TX errors, and no SDIO CRC or timeout messages. SHA-256: `8001b5facc144029cc1236d953a2f02b8026bcee039565ce08a5cd7e2d2e9064`.

DKMS automatically attempts to rebuild the fixed module for each new kernel when matching headers are installed. A future kernel API change can still require source adaptation, so keep the previous known-good kernel until the new one has been tested. Do not unload/reload this Wi-Fi module on a running affected machine; install it for the next boot and reboot cleanly.
