# Fortify SSC integration

* [ Code Dx Enterprise](https://github.com/snyk/user-docs/tree/58f91d848e16ddf2ffcca3711d6b8852412be402/hc/en-us/articles/360018848798--Code-Dx-Enterprise/README.md)
* [ Brinqa](https://github.com/snyk/user-docs/tree/58f91d848e16ddf2ffcca3711d6b8852412be402/hc/en-us/articles/360012728717-Brinqa/README.md)
* [ Fortify SSC integration](https://github.com/snyk/user-docs/tree/58f91d848e16ddf2ffcca3711d6b8852412be402/hc/en-us/articles/360005507838-Fortify-SSC-integration/README.md)
* [ Kenna Security](https://github.com/snyk/user-docs/tree/58f91d848e16ddf2ffcca3711d6b8852412be402/hc/en-us/articles/360013620217-Kenna-Security/README.md)
* [ Nucleus Security](https://github.com/snyk/user-docs/tree/58f91d848e16ddf2ffcca3711d6b8852412be402/hc/en-us/articles/360012502818-Nucleus-Security/README.md)
* [ RiskSense](https://github.com/snyk/user-docs/tree/58f91d848e16ddf2ffcca3711d6b8852412be402/hc/en-us/articles/360015069418-RiskSense/README.md)
* [ Vulcan-Cyber](https://github.com/snyk/user-docs/tree/58f91d848e16ddf2ffcca3711d6b8852412be402/hc/en-us/articles/360012981478-Vulcan-Cyber/README.md)

## Fortify SSC integration

Integrate the Snyk plugin with Micro Focus Fortify Software Security Center \(Fortify SSC\) and obtain a unified view of your open source security vulnerabilities.

Combining the two sources provides a more accurate view of the overall application portfolio security posture, and also naturally tracks that posture over time as vulnerabilities are fixed or introduced.

The Snyk parser plugin converts your Snyk scan results into a format that Fortify SSC can read and display.

### Fortify SSC integration: how it works

The Snyk plugin parses scanned results from Snyk and then feeds those results into Fortify SSC. In this way, you can view, monitor and manage your open source vulnerabilities in a single view.

To display Snyk data from the Fortify app:

1. The user runs a Snyk scan on a project from the CLI, generating a .json report.
2. The user uploads the report to Fortify SSC.
3. The plugin parses the results and feeds them to Fortify, for the application project.
4. The Snyk scan results are displayed from Fortify and the user can view and track data from the Fortify SSC app user interface \(UI\).
5. Ensure you have installed Snyk CLI and authenticated your account.
6. `SNYK_TOKEN` \(for use with our CLI\)—you can find your API token in your [account settings on snyk.io](https://app.snyk.io/account/)
7. Fortify SSC 18.20

### Fortify SSC and Snyk: an overview of the SSC feed

Once Snyk data is imported to the Fortify SSC app, navigate to the Audit tab to view the data. Each row of imported Snyk data is displayed in the Fortify feed with the Analysis Type SNYK and appears similar to the following image:

Expanding a specific vulnerability reveals detailed information as in the following example:
