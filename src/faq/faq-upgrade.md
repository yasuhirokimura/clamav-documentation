# Upgrading ClamAV

## ClamAV from Packages

If you installed from a package, we suggest you find the approved package from your distro provider and install that. The ClamAV team does not maintain individual packages for every distribution build.
If there are no new packages, you have three options:

* Wait
* Build ClamAV Package
* Install ClamAV From Source

## Install ClamAV From Source

If you installed from sources, first [uninstall the old version](faq-uninstall.md).

Then, [compile and install the new one](../manual/Installing/Installing-from-source-Unix.md).

Depending on your installation method, you might want to backup configuration (located in `/usr/local/etc` by default) and signature database (located in `/usr/local/share/clamav` by default). Don't forget to restore backups before starting up updated ClamAV.

Backup your database signature (located in `/usr/local/share/clamav` by default) before upgrading to newer ClamAV version. Restore the backed up database signature before running the updated version. This is to avoid getting the `/usr/local/share/clamav not locked` error message when doing `freshclam`.

## Webmin and Yum

To obtain a new version:

```bash
yum list clamav

yum update clamav
```

If everything updated properly, run `freshclam` to update your signature database.

### What does "*WARNING:	Current functionality level = 1, required = 2*" mean?

The *functionality level* of the database determines which scanner engine version is required to use all of its signatures. If you don't upgrade immediately you will be missing the latest viruses.

### What does "*Your ClamAV installation is OUTDATED*" mean?

You'll get this message whenever a new version of ClamAV is released. In order to detect all the latest viruses, it's not enough to keep your database up to date. You also need to run the latest version of the scanner. You can download the [sources] of the latest release from our website. If you are afraid to break something while upgrading, use  the [pre-compiled packages] for your operating system/distribution. Remember: running the latest stable release also improves stability.

### I upgraded to the latest stable version but I still get the message "*Your ClamAV installation is OUTDATED*", why?

Make sure there is really only one version of ClamAV installed on your system:

```bash
whereis freshclam

whereis clamscan
```

Also make sure that you haven't got old libraries (`libclamav.so*`) lying around your filesystem. You can verify it using:

```bash
ldd $(which freshclam)
```

### What does "*Malformed hexstring: This ClamAV version has reached End of Life*" mean?

Please refer to our [End-of-Life (EOL) policy](faq-eol.md).

### How do I verify the integrity of ClamAV sources?

You can verify the authenticity of ClamAV release packages provided [on the ClamAV downloads page](https://www.clamav.net/downloads) using [GnuPG](http://www.gnupg.org/).

1. Install GnuPG.
2. Download the [Cisco Talos GPG public key](https://raw.githubusercontent.com/Cisco-Talos/clamav-documentation/main/src/manual/cisco-talos.gpg).
3. Import the key into your local public keyring:
   ```bash
   gpg --import cisco-talos.gpg
   ```
4. Download the ClamAV package AND the corresponding `.sig` file to the same directory.
5. Verify that the stable release download is signed with the [Cisco Talos GPG public key]:
   ```bash
   gpg --verify clamav-X.XX.tar.gz.sig
   ```

The resulting output should look something like this. The specific details will differ, as we rotate the GPG key every couple of years:
```bash
    gpg: Signature made Wed Jan 24 19:31:26 2018 EST
    gpg:                using RSA key F13F9E16BCA5BFAD
    gpg: Good signature from "Talos (Talos, Cisco Systems Inc.) [email address]" [unknown]
```

### Where can I get the latest release, beta, or release candidate of ClamAV?

Visit the [source download page].

### Is my compiler/hardware/operating system supported by ClamAV?

ClamAV supports a wide variety of compilers, hardware and operating systems. Our core compiler is GCC with Linux on 32 and 64 bit Intel platforms, LLVM/Clang on macOS, and MSVC on Windows.


[sources]: https://github.com/Cisco-Talos/clamav
[pre-compiled packages]: http://www.clamav.net/download.html#otherversions
[source download page]: http://www.clamav.net/downloads
