# go-demo
go demo app


### Verify Attestation

Cosign Verify 
```bash
cosign verify-attestation --attachment-tag-prefix trivy ghcr.io/strongjz/go-demo@sha256:8609e13f162895bbee7840c0ab1b709ed67ddf4458c75abe2277fbd7bbc8f719 | jq -r .payload | base64 -d | jq -r .
```

or Rekor CLI, the rekor index is from the Trivy attestation step in GitHub Actions [here](https://github.com/strongjz/go-demo/runs/7235393067?check_suite_focus=true#step:11:386)
```bash
rekor-cli get --log-index 2869438 --format json | jq -r .Attestation | jq -r .
```

<details>

```json
{
  "_type": "https://in-toto.io/Statement/v0.1",
  "predicateType": "cosign.sigstore.dev/attestation/vuln/v1",
  "subject": [
    {
      "name": "ghcr.io/strongjz/go-demo",
      "digest": {
        "sha256": "8609e13f162895bbee7840c0ab1b709ed67ddf4458c75abe2277fbd7bbc8f719"
      }
    }
  ],
  "predicate": {
    "invocation": {
      "parameters": null,
      "uri": "https://github.com/strongjz/go-demo/actions/runs/2630238342",
      "event_id": "",
      "builder.id": "Release Latest Changes"
    },
    "scanner": {
      "uri": "https://github.com/aquasecurity/trivy",
      "version": "0.29.2",
      "db": {
        "uri": "",
        "version": ""
      },
      "result": {
        "$schema": "https://json.schemastore.org/sarif-2.1.0-rtm.5.json",
        "runs": [
          {
            "columnKind": "utf16CodeUnits",
            "originalUriBaseIds": {
              "ROOTPATH": {
                "uri": "file:///"
              }
            },
            "results": [],
            "tool": {
              "driver": {
                "fullName": "Trivy Vulnerability Scanner",
                "informationUri": "https://github.com/aquasecurity/trivy",
                "name": "Trivy",
                "rules": [],
                "version": "0.29.2"
              }
            }
          }
        ],
        "version": "2.1.0"
      }
    },
    "metadata": {
      "scanStartedOn": "2022-07-07T14:27:03Z",
      "scanFinishedOn": "2022-07-07T14:27:07Z"
    }
  }
}
```

</details>

### Grype Scan and Attestation

```bash
cosign verify-attestation --attachment-tag-prefix grype- ghcr.io/strongjz/go-demo@sha256:8609e13f162895bbee7840c0ab1b709ed67ddf4458c75abe2277fbd7bbc8f719 | jq -r .payload | base64 -d | jq -r .
```

[Rekor index from the GitHub Action](https://github.com/strongjz/go-demo/runs/7238089696?check_suite_focus=true#step:11:488)

```bash
rekor-cli get --log-index 2872800 --format json | jq -r .Attestation | jq -r .
```

<details>

```json
{
  "_type": "https://in-toto.io/Statement/v0.1",
  "predicateType": "cosign.sigstore.dev/attestation/vuln/v1",
  "subject": [
    {
      "name": "ghcr.io/strongjz/go-demo",
      "digest": {
        "sha256": "8609e13f162895bbee7840c0ab1b709ed67ddf4458c75abe2277fbd7bbc8f719"
      }
    }
  ],
  "predicate": {
    "invocation": {
      "parameters": null,
      "uri": "https://github.com/strongjz/go-demo/actions/runs/2631113049",
      "event_id": "",
      "builder.id": "Release Latest Changes"
    },
    "scanner": {
      "uri": "https://github.com/anchore/grype",
      "version": "0.38.0",
      "db": {
        "uri": "",
        "version": ""
      },
      "result": {
        "$schema": "https://json.schemastore.org/sarif-2.1.0-rtm.5.json",
        "runs": [
          {
            "results": [],
            "tool": {
              "driver": {
                "informationUri": "https://github.com/anchore/grype",
                "name": "Grype",
                "version": "0.38.0"
              }
            }
          }
        ],
        "version": "2.1.0"
      }
    },
    "metadata": {
      "scanStartedOn": "2022-07-07T17:01:44Z",
      "scanFinishedOn": "2022-07-07T17:02:00Z"
    }
  }
}
```

</details>
