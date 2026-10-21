# Colab distribution archive

The original `license_plate_colab.zip` is stored in 37 ordered chunks of at most
90 MiB. These are ordinary Git files; Git LFS is not required.

After cloning, reassemble the ZIP from the repository root.

## Windows PowerShell

```powershell
$parts = @(Get-ChildItem 'dist/license_plate_colab.zip.chunk???' | Sort-Object Name)
if ($parts.Count -ne 37) { throw 'Expected 37 archive chunks.' }
$archivePath = Join-Path (Get-Location) 'dist/license_plate_colab.zip'
$archiveStream = [System.IO.File]::Open($archivePath, [System.IO.FileMode]::CreateNew)
try {
    foreach ($part in $parts) {
        $partStream = [System.IO.File]::OpenRead($part.FullName)
        try { $partStream.CopyTo($archiveStream) }
        finally { $partStream.Dispose() }
    }
} finally { $archiveStream.Dispose() }
Get-FileHash -LiteralPath $archivePath -Algorithm SHA256
```

## Linux or macOS

```sh
cd dist
cat license_plate_colab.zip.chunk??? > license_plate_colab.zip
# Linux:
sha256sum -c license_plate_colab.zip.sha256
# macOS:
shasum -a 256 -c license_plate_colab.zip.sha256
```

The reconstructed ZIP must contain **3,491,505,573 bytes** and have SHA-256:

```text
7c00a36dbaed4e960044f9e46e9e89a8618450d89a1a8a15510ae4331e4e19b7
```

`license_plate_colab.zip.parts.sha256` lists checksums for the individual chunks.
