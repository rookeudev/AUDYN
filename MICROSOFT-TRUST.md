# Microsoft Trust and Distribution Status

## Current status

Audyn 0.5.0 is prepared for Authenticode signing and Microsoft Store submission, but a build must not be described as **Microsoft certified**, **Microsoft signed**, or **verified publisher** unless that exact build has completed the corresponding external process.

The application checks its own executable signature and displays the result in **Privacy center**:

- **AUTHENTICODE VERIFIED** means Windows validated a trusted publisher signature on that executable;
- **SIGNING PENDING** means the executable is not currently trusted as a signed publisher build.

A SHA-256 checksum verifies file integrity. It does not replace a trusted publisher signature and does not constitute Microsoft certification.

## Recommended Microsoft Store path

Microsoft recommends MSIX for most new Store submissions. A Store-published MSIX is signed by Microsoft after it passes certification. Completing that process requires action by the project owner:

1. Create a Windows developer account in Partner Center.
2. Reserve the Audyn product name.
3. Create an MSIX package and Store listing.
4. Provide the public privacy notice, descriptions, icons, and screenshots.
5. Submit the package for Microsoft certification.
6. Claim Store certification only after Partner Center reports that the submission passed.

Official Microsoft guidance:

- [Get started with Microsoft Store](https://learn.microsoft.com/windows/apps/publish/get-started)
- [Publish your first Windows app](https://learn.microsoft.com/windows/apps/package-and-deploy/publish-first-app)
- [Code-signing options for Windows developers](https://learn.microsoft.com/windows/apps/package-and-deploy/code-signing-options)

## Signing direct GitHub releases

For distribution outside Microsoft Store, use a valid organization-validation certificate or Microsoft Artifact Signing. Self-signed certificates are for development only and should not be presented as public trust.

When a trusted certificate is installed in the Windows certificate store, Audyn can be signed with:

```powershell
.\sign-release.ps1 `
  -CertificateThumbprint 0123456789ABCDEF0123456789ABCDEF01234567 `
  -TimestampUrl https://your-certificate-authority.example/timestamp
```

The script uses Windows SDK SignTool with SHA-256, adds a trusted timestamp, and verifies the resulting Authenticode signature. Protect the signing identity and never commit certificates, private keys, passwords, or signing service credentials to the repository.

Signing identifies the publisher and protects file integrity. It does not make source code impossible to recover and does not prove that software is free of vulnerabilities.

