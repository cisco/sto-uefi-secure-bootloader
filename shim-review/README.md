This repo is for review of requests for signing shim.  To create a request for review:

- clone this repo (preferably fork it)
- edit the template below
- add the shim.efi to be signed
- add build logs
- add any additional binaries/certificates/SHA256 hashes that may be needed
- commit all of that
- tag it with a tag of the form "myorg-shim-arch-YYYYMMDD"
- push it to GitHub
- file an issue at https://github.com/rhboot/shim-review/issues with a link to your tag
- approval is ready when the "accepted" label is added to your issue

Note that we really only have experience with using GRUB2 or systemd-boot on Linux, so
asking us to endorse anything else for signing is going to require some convincing on
your part.

As of 27 June 2026, shims sent to Microsoft can only be signed by the Microsoft UEFI CA 2023. It is no longer possible to get your shim signed by the "old" Microsoft Corporation UEFI CA 2011 key. Up-to-date information from Microsoft about Secure Boot can be found here: https://support.microsoft.com/en-US/servicing/os/secure-boot/2026/02/updates-and-announcements

New signing requirements have also taken effect, and are available here: https://techcommunity.microsoft.com/blog/hardware-dev-center/updated-microsoft-uefi-signing-requirements/1062916 Please note that undergoing this shim review exempts you from yearly security audits, as long as your shim only hands off to open source boot loaders.

Hint: check the [docs](./docs/) directory in this repo for guidance on submission and getting your shim signed.

Here's the template:

*******************************************************************************
### What organization or people are asking to have this signed?
*******************************************************************************
Organization name and website:

Cisco Systems - https://www.cisco.com

*******************************************************************************
### What's the legal data that proves the organization's genuineness?
The reviewers should be able to easily verify, that your organization is a legal entity, to prevent abuse.
Provide the information, which can prove the genuineness with certainty.
*******************************************************************************
Company/tax register entries or equivalent:
(a link to the organization entry in your jurisdiction's register will do)

Cisco Systems, Inc, IRS EIN Tax ID 77-0059951

Cisco Systems Annual reports filed with the SEC:
https://www.sec.gov/edgar/browse/?CIK=858877

Latest 10-K shows EIN:
https://www.sec.gov/ix?doc=/Archives/edgar/data/0000858877/000085887725000111/csco-20250726.htm

The public details of both your organization and the issuer in the EV certificate used for signing .cab files at Microsoft Hardware Dev Center File Signing Services.
(**not** the CA certificate embedded in your shim binary)

Example:

```
Issuer: O=MyIssuer, Ltd., CN=MyIssuer EV Code Signing CA
Subject: C=XX, O=MyCompany, Inc., CN=MyCompany, Inc.
```

Issuer: CN=DigiCert Trusted G4 Code Signing RSA4096 SHA384 2021 CA1, O="DigiCert, Inc.", C=US

Subject: CN="CISCO SYSTEMS, INC.", O="CISCO SYSTEMS, INC.", L=San Jose, ST=California, C=US, SERIALNUMBER=3704171

*******************************************************************************
### What product or service is this for?
*******************************************************************************
Cisco Appliances and Virtual products using Linux based Operating Systems.

*******************************************************************************
### What's the justification that this really does need to be signed for the whole world to be able to boot it?
*******************************************************************************
Customers using Cisco Virtual products that run on 3rd party servers (DELL, HP...) do not have permissions to change the UEFI db and add our keys. The Microsoft signed SHIM allows these products to securely run on those platforms.

*******************************************************************************
### Why are you unable to reuse shim from another distro that is already signed?
*******************************************************************************
We modify kernel configurations to meet our security requirements which requires signing with our own key.

*******************************************************************************
### Who is the primary contact for security updates, etc.?
The security contacts need to be verified before the shim can be accepted. For subsequent requests, contact verification is only necessary if the security contacts or their PGP keys have changed since the last successful verification.

An authorized reviewer will initiate contact verification by sending each security contact a PGP-encrypted email containing random words.
You will be asked to post the contents of these mails in your `shim-review` issue to prove ownership of the email addresses and PGP keys.
Please upload the PGP keys to a well-known keyserver like keyserver.ubuntu.com and/or include them in the review as an .asc file, and point to them here.

*******************************************************************************
- Name: Bridget Davis
- Position: Software Engineer
- Email address: briddavi@cisco.com
- PGP key fingerprint: 325F B75C CDCC 97CE F2AB  CF06 F805 AF99 E052 3CFB
- File/keyserver location: https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x325FB75CCDCC97CEF2ABCF06F805AF99E0523CFB

(Key should be signed by the other security contacts, pushed to a keyserver
like keyserver.ubuntu.com, and preferably have signatures that are reasonably
well known in the Linux community.)

*******************************************************************************
### Who is the secondary contact for security updates, etc.?
*******************************************************************************
- Name: Van Nguyen
- Position: Technical Leader
- Email address: vannguye@cisco.com
- PGP key fingerprint: 6A1D D8C5 0A9F 1B65 AF21  7B61 F0CB 4E57 37E9 C5DE
- File/keyserver location: https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x6A1DD8C50A9F1B65AF217B61F0CB4E5737E9C5DE

(Key should be signed by the other security contacts, pushed to a keyserver
like keyserver.ubuntu.com, and preferably have signatures that are reasonably
well known in the Linux community.)

*******************************************************************************
### Were these binaries created from the 16.1 shim release tar?
Please create your shim binaries starting with the 16.1 shim release tar file: https://github.com/rhboot/shim/releases/download/16.1/shim-16.1.tar.bz2

This matches https://github.com/rhboot/shim/releases/tag/16.1 and contains the appropriate gnu-efi source.

Make sure the tarball is correct by verifying your download's checksum
(SHA256, SHA512) with the following ones:

```
46319cd228d8f2c06c744241c0f342412329a7c630436fce7f82cf6936b1d603  shim-16.1.tar.bz2
ca5f80e82f3b80b622028f03ef23105c98ee1b6a25f52a59c823080a3202dd4b9962266489296e99f955eb92e36ce13e0b1d57f688350006bba45f2718f159fb  shim-16.1.tar.bz2
```

Make sure that you've verified that your build process uses that file
as a source of truth (excluding external patches) and its checksum
matches. You can also further validate the release by checking the PGP
signature: there's [a detached
signature](https://github.com/rhboot/shim/releases/download/16.1/shim-16.1.tar.bz2.asc)

The release is signed by the maintainer Peter Jones - his master key
has the fingerprint `B00B48BC731AA8840FED9FB0EED266B70F4FEF10` and the
signing sub-key in the signature here has the fingerprint
`02093E0D19DDE0F7DFFBB53C1FD3F540256A1372`. A copy of his public key
is included here for reference:
[pjones.asc](https://github.com/rhboot/shim-review/blob/main/pjones.asc)

Once you're sure that the tarball you are using is correct and
authentic, please confirm this here with a simple *yes*.

A short guide on verifying public keys and signatures should be available in the [docs](./docs/) directory.
*******************************************************************************
Yes. We created the shim binaries from the 16.1 shim release tarball at https://github.com/rhboot/shim/releases/download/16.1/shim-16.1.tar.bz2 (see the Dockerfile), and its checksum matches the values above.

*******************************************************************************
### URL for a repo that contains the exact code which was built to result in your binary:
Hint: If you attach all the patches and modifications that are being used to your application, you can point to the URL of your application here (*`https://github.com/YOUR_ORGANIZATION/shim-review`*).

You can also point to your custom git servers, where the code is hosted.
*******************************************************************************
https://github.com/cisco/sto-uefi-secure-bootloader/tree/rel_7/shim-review

*******************************************************************************
### What patches are being applied and why:
Mention all the external patches and build process modifications, which are used during your building process, that make your shim binary be the exact one that you posted as part of this application.
*******************************************************************************
None

*******************************************************************************
### Do you have the NX bit set in your shim? If so, is your entire boot stack NX-compatible and what testing have you done to ensure such compatibility?

See https://techcommunity.microsoft.com/t5/hardware-dev-center/nx-exception-for-shim-community/ba-p/3976522 for more details on the signing of shim without NX bit.
*******************************************************************************
NX bit is not set. We tested it on our target x86 architecture with entire shim/grub bootchain.

*******************************************************************************
### What exact implementation of Secure Boot in GRUB2 do you have? (Either Upstream GRUB2 shim_lock verifier or Downstream RHEL/Fedora/Debian/Canonical-like implementation)
Skip this, if you're not using GRUB2.
*******************************************************************************
Using downstream implementations from Almalinux and Canonical.

*******************************************************************************
### Do you have fixes for all the following GRUB2 CVEs applied?
**Skip this, if you're not using GRUB2, otherwise make sure these are present and confirm with _yes_.**

* 2020 July - BootHole
  * Details: https://lists.gnu.org/archive/html/grub-devel/2020-07/msg00034.html
  * CVE-2020-10713
  * CVE-2020-14308
  * CVE-2020-14309
  * CVE-2020-14310
  * CVE-2020-14311
  * CVE-2020-15705
  * CVE-2020-15706
  * CVE-2020-15707
* March 2021
  * Details: https://lists.gnu.org/archive/html/grub-devel/2021-03/msg00007.html
  * CVE-2020-14372
  * CVE-2020-25632
  * CVE-2020-25647
  * CVE-2020-27749
  * CVE-2020-27779
  * CVE-2021-3418 (if you are shipping the shim_lock module)
  * CVE-2021-20225
  * CVE-2021-20233
* June 2022
  * Details: https://lists.gnu.org/archive/html/grub-devel/2022-06/msg00035.html, SBAT increase to 2
  * CVE-2021-3695
  * CVE-2021-3696
  * CVE-2021-3697
  * CVE-2022-28733
  * CVE-2022-28734
  * CVE-2022-28735
  * CVE-2022-28736
  * CVE-2022-28737
* November 2022
  * Details: https://lists.gnu.org/archive/html/grub-devel/2022-11/msg00059.html, SBAT increase to 3
  * CVE-2022-2601
  * CVE-2022-3775
* October 2023 - NTFS vulnerabilities
  * Details: https://lists.gnu.org/archive/html/grub-devel/2023-10/msg00028.html, SBAT increase to 4
  * CVE-2023-4693
  * CVE-2023-4692
* February 2025
  * Details: https://lists.gnu.org/archive/html/grub-devel/2025-02/msg00024.html, SBAT increase to 5
  * CVE-2024-45774
  * CVE-2024-45775
  * CVE-2024-45776
  * CVE-2024-45777
  * CVE-2024-45778
  * CVE-2024-45779
  * CVE-2024-45780
  * CVE-2024-45781
  * CVE-2024-45782
  * CVE-2024-45783
  * CVE-2025-0622
  * CVE-2025-0624
  * CVE-2025-0677
  * CVE-2025-0678
  * CVE-2025-0684
  * CVE-2025-0685
  * CVE-2025-0686
  * CVE-2025-0689
  * CVE-2025-0690
  * CVE-2025-1118
  * CVE-2025-1125
*******************************************************************************
Yes. These CVEs are addressed in the parent distros. We do not modify the source of the grub.

*******************************************************************************
### If shim is loading GRUB2 bootloader, and if these fixes have been applied, is the upstream global SBAT generation in your GRUB2 binary set to 5?
Skip this, if you're not using GRUB2, otherwise do you have an entry in your GRUB2 binary similar to:
`grub,5,Free Software Foundation,grub,GRUB_UPSTREAM_VERSION,https://www.gnu.org/software/grub/`?
*******************************************************************************
Yes

*******************************************************************************
### Were old shims hashes provided to Microsoft for verification and to be added to future DBX updates?
### Does your new chain of trust disallow booting old GRUB2 builds affected by the CVEs?
If you had no previous signed shim, say so here. Otherwise a simple _yes_ will do.
*******************************************************************************
Yes they were provided. Yes, old GRUB2 builds affected by the CVEs are disallowed
due to building the shim with SBAT_AUTOMATIC_DATE=2025021800.

*******************************************************************************
### If your boot chain of trust includes a Linux kernel:
### Is upstream commit [1957a85b0032a81e6482ca4aab883643b8dae06e "efi: Restrict efivar_ssdt_load when the kernel is locked down"](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=1957a85b0032a81e6482ca4aab883643b8dae06e) applied?
### Is upstream commit [75b0cea7bf307f362057cc778efe89af4c615354 "ACPI: configfs: Disallow loading ACPI tables when locked down"](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=75b0cea7bf307f362057cc778efe89af4c615354) applied?
### Is upstream commit [eadb2f47a3ced5c64b23b90fd2a3463f63726066 "lockdown: also lock down previous kgdb use"](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=eadb2f47a3ced5c64b23b90fd2a3463f63726066) applied?
Hint: upstream kernels should have all these applied, but if you ship your own heavily-modified older kernel version, that is being maintained separately from upstream, this may not be the case.
If you are shipping an older kernel, double-check your sources; maybe you do not have all the patches, but ship a configuration, that does not expose the issue(s).
*******************************************************************************
Yes

*******************************************************************************
### How does your signed kernel enforce lockdown when your system runs with Secure Boot enabled?
Hint: If it does not, we are not likely to sign your shim.
*******************************************************************************
We will be enabling upstream kernel lockdown flags.

*******************************************************************************
### Do you build your signed kernel with additional local patches? What do they do?
*******************************************************************************
Our patches apply KSPP hardening configurations.

*******************************************************************************
### Do you use an ephemeral key for signing kernel modules?
### If not, please describe how you ensure that one kernel build does not load modules built for another kernel.
*******************************************************************************
We do not use an ephemeral key. We use a Cisco HSM backed key for signing.

*******************************************************************************
### If you use vendor_db functionality of providing multiple certificates and/or hashes please briefly describe your certificate setup.
### If there are allow-listed hashes please provide exact binaries for which hashes are created via file sharing service, available in public with anonymous access for verification.
*******************************************************************************
We do not use vendor_db

*******************************************************************************
### If you are re-using the CA certificate from your last shim binary, you will need to add the hashes of the previous GRUB2 binaries exposed to the CVEs mentioned earlier to vendor_dbx in shim. Please describe your strategy.
This ensures that your new shim+GRUB2 can no longer chainload those older GRUB2 binaries with issues.

If this is your first application or you're using a new CA certificate, please say so here.
*******************************************************************************
We re-use the cert from #411. We re-sign upstream GRUBs, those carry upstream grub,5 SBAT.
We build the shim with SBAT_AUTOMATIC_DATE=2025021800 so grub,5 is enforced.

*******************************************************************************
### Is the Dockerfile in your repository the recipe for reproducing the building of your shim binary?
A reviewer should always be able to run `docker build .` to get the exact binary you attached in your application.

Hint: Prefer using *frozen* packages for your toolchain, since an update to GCC, binutils, gnu-efi may result in building a shim binary with a different checksum.

If your shim binaries can't be reproduced using the provided Dockerfile, please explain why that's the case, what the differences would be and what build environment (OS and toolchain) is being used to reproduce this build? In this case please write a detailed guide, how to setup this build environment from scratch.
*******************************************************************************
Yes. The build is driven by the Dockerfile https://github.com/cisco/sto-uefi-secure-bootloader/blob/rel_7/shim-review/Dockerfile , invoked via the Makefile https://github.com/cisco/sto-uefi-secure-bootloader/blob/rel_7/shim-review/Makefile .

You can use

```
make build-no-cache
```

*******************************************************************************
### Which files in this repo are the logs for your build?
This should include logs for creating the buildroots, applying patches, doing the build, creating the archives, etc.
*******************************************************************************
The log file is: https://github.com/cisco/sto-uefi-secure-bootloader/blob/rel_7/shim-review/build.log

*******************************************************************************
### What changes were made in the distro's secure boot chain since your SHIM was last signed?
For example, signing new kernel's variants, UKI, systemd-boot, new certs, new CA, etc..

Skip this, if this is your first application for having shim signed.
*******************************************************************************
Rebased against 16.1

*******************************************************************************
### What is the SHA256 hash of your final shim binary?
*******************************************************************************
06de30a7b6d20ee0dfa8433b10078a85553c2f2c282aafba5dd7153d46e5de2f

*******************************************************************************
### How do you manage and protect the keys used in your shim?
Describe the security strategy that is used for key protection. This can range from using hardware tokens like HSMs or Smartcards, air-gapped vaults, physical safes to other good practices.
*******************************************************************************
Our keys are in a Cisco HSM, accessible only by authorized members.

*******************************************************************************
### Do you use EV certificates as embedded certificates in the shim?
A _yes_ or _no_ will do. There's no penalty for the latter.
*******************************************************************************
No, we do not use EV certificates.

*******************************************************************************
### Are you embedding a CA certificate in your shim?
A _yes_ or _no_ will do. There's no penalty for the latter. However,
if _yes_: does that certificate include the X509v3 Basic Constraints
to say that it is a CA? See the [docs](./docs/) for more guidance
about this.
*******************************************************************************
Yes, we embed the Cisco_Virtual_UEFI_SubCA_v3.der CA certificate.
The certificate includes X509v3 Basic Constraints to say that it is a CA.

Regarding its long validity, we manage our expiry more granularly at the End Entity level. We prefer to keep the CAs at a longer time period to avoid issues with CA expiry. This was mentioned in our [previous review](https://github.com/rhboot/shim-review/issues/411#issuecomment-2165360503)

*******************************************************************************
### Do you add a vendor-specific SBAT entry to the SBAT section in each binary that supports SBAT metadata ( GRUB2, fwupd, fwupdate, systemd-boot, systemd-stub, shim + all child shim binaries )?
### Please provide the exact SBAT entries for all binaries you are booting directly through shim.
Hint: The history of SBAT and more information on how it works can be found [here](https://github.com/rhboot/shim/blob/main/SBAT.md). That document is large, so for just some examples check out [SBAT.example.md](https://github.com/rhboot/shim/blob/main/SBAT.example.md)

If you are using a downstream implementation of GRUB2 (e.g. from Fedora or Debian), make sure you have their SBAT entries preserved and that you **append** your own (don't replace theirs) to simplify revocation.

**Remember to post the entries of all the binaries. Apart from your bootloader, you may also be shipping e.g. a firmware updater, which will also have these.**

Hint: run `objcopy --dump-section .sbat=/dev/stdout YOUR_EFI_BINARY` to get these entries. Paste them here. Preferably surround each listing with three backticks (```), so they render well.
*******************************************************************************
shim/fb/mm:
```
sbat,1,SBAT Version,sbat,1,https://github.com/rhboot/shim/blob/main/SBAT.md
shim,4,UEFI shim,shim,1,https://github.com/rhboot/shim
shim.cisco,1,Cisco,shim,16.1,psirt@cisco.com
```

We use upstream distros for grub since we are not rebuilding it. They all contain `grub,5`.

*******************************************************************************
### If shim is loading GRUB2 bootloader, which modules are built into your signed GRUB2 image?
Skip this, if you're not using GRUB2.

Hint: this is about those modules that are in the binary itself, not the `.mod` files in your filesystem.
*******************************************************************************
We inherit the same modules from the upstream grub providers.

Ubuntu 24:
```
acpi              gcry_cast5        loadenv           raid5rec
afsplitter        gcry_crc          loopback          raid6rec
all_video         gcry_des          ls                reboot
archelp           gcry_dsa          lsefi             regexp
bitmap            gcry_idea         lsefimmap         relocator
bitmap_scale      gcry_md4          lsefisystab       search
boot              gcry_md5          lssal             search_fs_file
btrfs             gcry_rfc2268      luks              search_fs_uuid
bufio             gcry_rijndael     lvm               search_label
cat               gcry_rmd160       lzopio            serial
chain             gcry_rsa          mdraid09          setjmp
configfile        gcry_seed         mdraid1x          sleep
cpuid             gcry_serpent      memdisk           smbios
crypto            gcry_sha1         minicmd           squash4
cryptodisk        gcry_sha256       mmap              terminal
datetime          gcry_sha512       mpi               terminfo
disk              gcry_tiger        net               test
diskfilter        gcry_twofish      normal            tpm
echo              gcry_whirlpool    ntfs              trig
efi_gop           gettext           part_apple        true
efi_uga           gfxmenu           part_gpt          video
efifwsetup        gfxterm           part_msdos        video_bochs
efinet            gfxterm_background password_pbkdf2  video_cirrus
ext2              gzio              pbkdf2            video_colors
extcmd            halt              peimage           video_fb
fat               help              pgp               xfs
font              hfsplus           play              xzio
fshelp            iso9660           png               zfs
gcry_arcfour      jpeg              priority_queue    zfscrypt
gcry_blowfish     keystatus         probe             zfsinfo
gcry_camellia     linux             procfs            zstd
```

AlmaLinux 10:
```
acpi              extcmd            loopback          search
afsplitter        f2fs              lsefi             search_fs_file
all_video         fat               lsefimmap         search_fs_uuid
archelp           font              luks              search_label
at_keyboard       fshelp            luks2             serial
backtrace         gcry_crc          lvm               sleep
bitmap            gcry_keccak       lzopio            squash4
bitmap_scale      gcry_rijndael     mdraid09          syslinuxcfg
blscfg            gcry_rsa          mdraid1x          terminal
blsuki            gcry_serpent      memdisk           terminfo
boot              gcry_sha1         minicmd           test
btrfs             gcry_sha256       mmap              tftp
bufio             gcry_sha512       mpi               tpm
cat               gcry_twofish      net               trig
chain             gcry_whirlpool    normal            usb
configfile        gettext           part_apple        usbserial_common
connectefi        gfxmenu           part_gpt          usbserial_ftdi
crypto            gfxterm           part_msdos        usbserial_pl2303
cryptodisk        gzio              password_pbkdf2   usbserial_usbdebug
datetime          halt              pbkdf2            version
disk              hfsplus           pgp               video
diskfilter        http              png               video_bochs
echo              increment         priority_queue    video_cirrus
efi_gop           iso9660           procfs            video_colors
efi_netfs         jpeg              raid6rec          video_fb
efi_uga           json              reboot            xfs
efifwsetup        keylayouts        regexp            xzio
efinet            linux             relocator         zstd
ext2              loadenv
```

*******************************************************************************
### If you are using systemd-boot on arm64 or riscv, is the fix for [unverified Devicetree Blob loading](https://github.com/systemd/systemd/security/advisories/GHSA-6m6p-rjcq-334c) included?
*******************************************************************************
N/A

*******************************************************************************
### What is the origin and full version number of your bootloader (GRUB2 or systemd-boot or other)?
*******************************************************************************
We resign grub from upstream distros. The current versions are:

Ubuntu 22.04 (Jammy): grub2 - Version 2.06-2ubuntu14.8

Ubuntu 24.04 (Noble): grub2 - Version 2.12-1ubuntu7.3

AlmaLinux 9: grub2 - Version 2.06-126.el9_8.alma.1

AlmaLinux 10: grub2 - Version  2.12-46.el10_2.alma.1

*******************************************************************************
### If your shim launches any other components apart from your bootloader, please provide further details on what is launched.
Hint: The most common case here will be a firmware updater like fwupd.
*******************************************************************************
N/A

*******************************************************************************
### If your GRUB2 or systemd-boot launches any other binaries that are not the Linux kernel in SecureBoot mode, please provide further details on what is launched and how it enforces Secureboot lockdown.
Skip this, if you're not using GRUB2 or systemd-boot.
*******************************************************************************
N/A

*******************************************************************************
### How do the launched components prevent execution of unauthenticated code?
Summarize in one or two sentences, how your secure bootchain works on higher level.
*******************************************************************************
Shim -> Grub -> Kernel

shim validates grub2 against the embedded CA cert, grub2 asks shim to validate kernel signture

*******************************************************************************
### Does your shim load any loaders that support loading unsigned kernels (e.g. certain GRUB2 configurations)?
*******************************************************************************
No

*******************************************************************************
### What kernel are you using? Which patches and configuration does it include to enforce Secure Boot?
*******************************************************************************
Latest kernel version of Ubuntu and AlmaLinux. Versions current at submission; we track each distribution's security updates.

Ubuntu 22.04 (Jammy): linux-image-5.15.0-190-generic

Ubuntu 24.04 (Noble): linux-image-6.8.0-138-generic

AlmaLinux 9: kernel-5.14.0-687.39.1.el9_8

AlmaLinux 10: kernel-6.12.0-211.49.1.el10_2

Inherits Ubuntu and AlmaLinux's Secure Boot patches and configuration.

*******************************************************************************
### What contributions have you made to help us review the applications of other applicants?
The reviewing process is meant to be a peer-review effort and the best way to have your application reviewed faster is to help with reviewing others. We are in most cases volunteers working on this venue in our free time, rather than being employed and paid to review the applications during our business hours.

A reasonable timeframe of waiting for a review can reach 2-3 months. Helping us is the best way to shorten this period. The more help we get, the faster and the smoother things will go.

For newcomers, the applications labeled as [*easy to review*](https://github.com/rhboot/shim-review/issues?q=is%3Aopen+is%3Aissue+label%3A%22easy+to+review%22) are recommended to start the contribution process.
*******************************************************************************
I have performed 3 shim reviews:

https://github.com/rhboot/shim-review/issues/504#issuecomment-3444722678

https://github.com/rhboot/shim-review/issues/505#issuecomment-3427098529

https://github.com/rhboot/shim-review/issues/515#issuecomment-3959926003

*******************************************************************************
### Add any additional information you think we may need to validate this shim signing application.
*******************************************************************************
We use this SHIM to boot multiple bootloaders from distro vendors such as Canonical and Almalinux. This is because we modify kernel configs to apply additional security controls which require rebuilding and re-signing.
