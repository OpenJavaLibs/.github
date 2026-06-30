# Releasing `pojo-tester`

This document describes the manual process for publishing a new version of `pojo-tester` to Maven Central.

---

# Publishing Resources

## GitHub Repository

https://github.com/OpenJavaLibs/pojo-tester

## Maven Central Artifact

https://central.sonatype.com/artifact/io.github.openjavalibs/pojo-tester

## Sonatype Central

### Deployments

https://central.sonatype.com/publishing/deployments

### Namespaces

https://central.sonatype.com/publishing/namespaces

Current verified namespaces:

| Namespace | Status | Notes |
|-----------|--------|-------|
| `io.github.openjavalibs` | ✅ Verified | SNAPSHOTs enabled, Insights enabled |
| `io.github.srikanthkapila` | ✅ Verified | Personal namespace |

---

# Prerequisites

- GitHub repository is up to date.
- Sonatype Central account is linked to the GitHub account that owns `io.github.openjavalibs`.
- GPG key is already configured.
- Maven credentials are configured in `~/.m2/settings.xml`.

---

# 1. Create a Release Branch (Optional)

If releasing a bug fix:

```bash
git checkout -b release/1.2.1
```

Otherwise, release directly from `main`.

---

# 2. Update the Version

Update the version in `pom.xml`.

Example:

```xml
<version>1.2.1</version>
```

---

# 3. Commit the Changes

```bash
git add .
git commit -m "Release 1.2.1"
git push
```

---

# 4. Build Locally

```bash
mvn clean verify
```

Verify that:

- All tests pass.
- Sources JAR is generated.
- Javadoc JAR is generated.
- Artifacts are signed successfully.

---

# 5. Deploy

```bash
mvn clean deploy
```

Expected output:

```
Uploaded bundle successfully
deploymentId: <deployment-id>
```

---

# 6. Publish from Sonatype

Open:

https://central.sonatype.com/publishing/deployments

1. Wait until the deployment status becomes **VALIDATED**.
2. Click **Publish**.
3. Wait for the status to change to **PUBLISHED**.

---

# 7. Verify Publication

Verify the published artifact:

https://central.sonatype.com/artifact/io.github.openjavalibs/pojo-tester

---

# 8. Tag the Release

```bash
git tag v1.2.1
git push origin v1.2.1
```

---

# Troubleshooting

## GPG Error

```
gpg: signing failed: Inappropriate ioctl for device
```

### Fix

```bash
export GPG_TTY=$(tty)
```

Verify:

```bash
echo $GPG_TTY
```

Expected output:

```
/dev/ttys004
```

Retry:

```bash
mvn clean deploy
```

---

## Sonatype Plugin Error

```
Unrecognized field "warnings"
```

### Cause

An older version of the Sonatype Central Publishing Maven Plugin cannot parse the latest API response.

The artifact upload usually succeeds despite the build failure.

### Resolution

- Open the Deployments page.
- Verify the deployment exists.
- If the status is **VALIDATED**, publish it manually.
- Upgrade the `central-publishing-maven-plugin` before the next release.

---

## Version Already Exists

Maven Central versions are immutable.

Increase the version in `pom.xml` and deploy again.

---

# Useful Commands

```bash
git status

git branch

git checkout main

git pull

mvn clean verify

mvn clean deploy

git tag v1.2.1

git push origin v1.2.1
```
