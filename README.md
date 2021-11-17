# yarn-npm-audit-bug

Requires yarn 3. `npm install -g yarn` then `yarn set version berry` to set your
version to yarn 3.

This repository demonstrates a bug in yarn 3.1.0 and earlier, where npm security
advisories are incorrectly reported. There are two relevant branches:

- `vulnerability-reported`: This branch has an old version of lodash installed,
    which is affected by [this security
    advisory](https://github.com/advisories/GHSA-35jh-r3h4-6jhm). `yarn npm
    audit` correctly reports this vulnerability.
- `vulnerability-not-reported-version-overridden`: This branch also has the
    same old, vulnerable version of lodash installed. It also has the most
    recent version of `html-webpack-plugin` installed as a dev dependency, which
    itself installs a more recent version of lodash that is not vulnerable to
    this command injection vulnerability. While the proof-of-concept exploit
    still works, `yarn npm audit` does not report this vulnerability, likely due
    to the more recent version of lodash also being installed.

### Running the PoC

- Run `yarn install`
- Run `yarn node index.js` to demonstrate the command injection vulnerability in
    the installed version of lodash. The PoC dumps out the contents of
    `process.env` by way of a command injection vulnerability in an old version
    of lodash.

### Security Advisory Reporting


On the `vulnerability-reported` branch, running a security audit will return the
following vulnerability:

```
$ yarn npm audit

└─ lodash: 4.17.20
   ├─ Issue: Command Injection in lodash
   ├─ URL: https://github.com/advisories/GHSA-35jh-r3h4-6jhm
   ├─ Severity: high
   ├─ Vulnerable Versions: <4.17.21
   ├─ Patched Versions: >=4.17.21
   ├─ Via: lodash
   └─ Recommendation: Upgrade to version 4.17.21 or later
 ```

On the `vulnerability-not-reported-version-overridden` branch, running a
security audit will return no vulnerabilities, but the PoC described above will
still execute:


```
$ yarn npm audit

➤ YN0001: No audit suggestions
```
