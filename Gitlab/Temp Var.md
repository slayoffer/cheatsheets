```yaml
variables:
  ENV_UPC: "TEST"
  TEST_CA_PEM: "-----BEGIN CERTIFICATE----- ... -----END CERTIFICATE-----"

my_job:
  script:
    - export TMP="$ENV_UPC"_CA_PEM
    - export CA_PEM_VALUE=$(eval echo "\$${TMP}")
    - echo "The value of CA_PEM is: $CA_PEM_VALUE"
```