# yarn-npm-audit-bug

Requires yarn 3. `npm install -g yarn` then `yarn set version berry` to set your
version to yarn 3.

### Running the PoC

- Run `yarn install`
- Run `yarn node index.js` to demonstrate the command injection vulnerability in
    the installed version of lodash. The PoC dumps out the contents of
    `process.env` by way of a command injection vulnerability in an old version
    of lodash.

### Security Advisory Reporting


On the `vulnerability-reported` branch, running a security audit will return the
following vulnerability:

$ yarn npm audit

└─ lodash: 4.17.20
   ├─ Issue: Command Injection in lodash
   ├─ URL: https://github.com/advisories/GHSA-35jh-r3h4-6jhm
   ├─ Severity: high
   ├─ Vulnerable Versions: <4.17.21
   ├─ Patched Versions: >=4.17.21
   ├─ Via: lodash
   └─ Recommendation: Upgrade to version 4.17.21 or later

On the `vulnerability-not-reported-version-overridden` branch, running a
security audit will return no vulnerabilities, but the PoC described above will
still execute.

