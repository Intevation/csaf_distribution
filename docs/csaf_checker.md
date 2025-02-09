## csaf_checker

### Usage

```
Usage:
  csaf_checker [OPTIONS] domain...

Application Options:
  -o, --output=REPORT-FILE       File name of the generated report
  -f, --format=[json|html]       Format of report (default: json)
      --insecure                 Do not check TLS certificates from provider
      --client-cert=CERT-FILE    TLS client certificate file (PEM encoded data)
      --client-key=KEY-FILE      TLS client private key file (PEM encoded data)
      --version                  Display version of the binary
  -v, --verbose                  Verbose output
  -r, --rate=                    The average upper limit of https operations per second (defaults to unlimited)
  -y, --years=YEARS              Number of years to look back from now
  -H, --header=                  One or more extra HTTP header fields
      --validator=URL            URL to validate documents remotely
      --validatorcache=FILE      FILE to cache remote validations
      --validatorpreset=         One or more presets to validate remotely (default: mandatory)


Help Options:
  -h, --help                     Show this help message
```

Will check all given _domains_, by trying each as a CSAF provider.

If a _domain_ starts with `https://` it is instead considered a direct URL to the `provider-metadata.json` and checking proceeds from there.


Usage example:
` ./csaf_checker example.com -f html --rate=5.3 -H apikey:SECRET -o check-results.html`

Each performed check has a return type of either 0,1 or 2:
```
type 0: success
type 1: warning
type 2: error
```

The checker result is a success if no checks resulted in type 2, and a failure otherwise. 


### Remarks

The `role` given in the `provider-metadata.json` is not
yet considered to change the overall result,
see https://github.com/csaf-poc/csaf_distribution/issues/221 .

If a provider hosts one or more advisories with a TLP level of AMBER or RED, then these advisories must be access protected.
To check these advisories, authorization can be given via custom headers or certificates.
The authorization method chosen needs to grant access to all advisories, as otherwise the
checker will be unable to check the advisories it doesn't have permission for, falsifying the result.
