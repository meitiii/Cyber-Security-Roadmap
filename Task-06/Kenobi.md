# 🛡️ Kenobi — TryHackMe (Summary)

This room focuses on exploiting Samba shares, leveraging vulnerabilities in FTP servers (`ProFTPD`) to manipulate filesystem paths, and performing privilege escalation via abused SUID binaries and PATH variable manipulation.

---

## 🔍 Enumeration

- **Network Scanning:** Use `nmap` to identify open ports, specifically searching for SMB (445/139), FTP (21), and RPC (111).
- **SMB Enumeration:** Use `nmap --script=smb-enum-shares.nse,smb-enum-users.nse` to find accessible shares.
- **Accessing SMB:** Use `smbclient //[TARGET_IP]/anonymous` (no password required) to list and download files (e.g., `log.txt`).
- **NFS Enumeration:** Use `nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount [TARGET_IP]` to identify mountable network file systems.

---

## 🛠️ Exploitation: ProFTPD & mod_copy

The target runs a vulnerable version of `ProFTPD` (1.3.5) with the `mod_copy` module enabled, which allows unauthenticated file movement.

1. **Version Identification:** Use `nc [IP] 21` to verify the service version.
2. **Exploitation:** Since we know the Kenobi user's private SSH key (`id_rsa`) exists, we use `SITE CPFR` and `SITE CPTO` commands to copy the key into the `/var/tmp` directory (which is both writable and mountable).
3. **Retrieval:** Mount the `/var` share locally:
```bash
   mount [TARGET_IP]:/var /mnt/kenobiNFS