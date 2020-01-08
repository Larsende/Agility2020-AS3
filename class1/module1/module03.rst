Lab 2 - Creating an HTTPS application with SSL Offload or SSL Bridging
----------------------------------------------------------------------

#. In this section we will start by building out a basic HTTPS application with SSL Offload.  
#. In Postman expand the section for Lab3.
#. Open the ``BIG-IP: Basic HTTPS Load Balancing`` option and click on the ``Body`` section of the POST Request.

The body will look like the following:

    .. code-block:: json
        :linenos:

        {
          "class": "AS3",
          "action": "deploy",
          "persist": true,
          "declaration": {
            "class": "ADC",
            "schemaVersion": "3.0.0",
            "id": "123abc",
            "label": "Sample 2",
            "remark": "HTTPS with predictive-node pool",
            "Sample_02": {
              "class": "Tenant",
              "A1": {
                "class": "Application",
                "template": "https",
                "serviceMain": {
                  "class": "Service_HTTPS",
                  "virtualAddresses": [
                    "10.1.10.45"
                  ],
                  "pool": "web_pool",
                  "serverTLS": "webtls"
                },
                "web_pool": {
                  "class": "Pool",
                  "loadBalancingMode": "predictive-node",
                  "monitors": [
                   "http"
                  ],
                  "members": [{
                    "servicePort": 80,
                    "shareNodes": true,
                    "serverAddresses": [
                      "10.1.10.31",
                      "10.1.10.32"
                    ]
                  }]
                },
                "webtls": {
                  "class": "TLS_Server",
                  "certificates": [{
                    "certificate": "webcert"
                  }]
                },
                "webcert": {
                  "class": "Certificate",
                  "remark": "in practice we recommend using a passphrase",
                  "certificate": "-----BEGIN CERTIFICATE-----\MIIC8jCCAdqgAwIBAgIJAKQOdKoHQWwwMA0GCSqGSIb3DQEBCwUAMDExLzAtBgNV\BAMMJmlwLTEwLTEtMS00LnVzLXdlc3QtMi5jb21wdXRlLmludGVybmFsMB4XDTE3\MDMyNzE0MzY1MVoXDTI3MDMyNTE0MzY1MVowMTEvMC0GA1UEAwwmaXAtMTAtMS0x\LTQudXMtd2VzdC0yLmNvbXB1dGUuaW50ZXJuYWwwggEiMA0GCSqGSIb3DQEBAQUA\A4IBDwAwggEKAoIBAQCfZmtyGhTH2nXpzczMwOPLQx6lcoqla/h2F/TZd+hSEtra\I+GSAYpqrrT4w03u9pI3u3xpW4DUCYUijyLw9WfkjEgb9/gommkXA5oyrLAJPgFe\KHUTrK21gsc6Aw6X4ewArxMjcgtPkKWqih9rupwS6/iPjqHQBJ5d6Io120V6KaSv\GhbARcyhMkMH+wp9qG7Ica1CFVqZz51XWyaq/tvuR71tCXRtaryvQ8J4LCtYssoY\YEklfU5sTX1CFRtN15bBr/KLH/1lhZu6InEOImltDxJ7xNXh1g/EI3RZfLjmXnIf\REq+ype4CaI04KIBNthpA72i0WTb/vQI5WSC1UV/AgMBAAGjDTALMAkGA1UdEwQC\MAAwDQYJKoZIhvcNAQELBQADggEBADX+1PFBVJu260HbshB/yJV6NUL+m4+S1ux1\EwEve39UHW3z/UUJu9V3Gli9DxF/5J5CY7TPl9w+Vj5/budrm4A45p5aoXDU4J3e\ZYcYcAJmvwGzFleuMImO3cLOIoAvGt2ztQ6yo9xNICutz/fmxZbhwBK08dSluShS\B/3lqfQgBEIjtXqTepbcycPmxx0adnEVD5ObDUKPE0nvm8wSg17+E/DNHdKsbbcR\3uqbq19PSafrZ5YQfzkCqJb7weLc9O5tfagunDwFVDsArbvgTrHJ6io8o2HhdjS8\Sem0nZlzapnhxzqa0jz9fRKJhYidyHt6B+/Vj8Owocvc023CIrY=\-----END CERTIFICATE-----",
                  "privateKey": "-----BEGIN PRIVATE KEY-----\MIIEvAIBADANBgkqhkiG9w0BAQEFAASCBKYwggSiAgEAAoIBAQCfZmtyGhTH2nXp\zczMwOPLQx6lcoqla/h2F/TZd+hSEtraI+GSAYpqrrT4w03u9pI3u3xpW4DUCYUi\jyLw9WfkjEgb9/gommkXA5oyrLAJPgFeKHUTrK21gsc6Aw6X4ewArxMjcgtPkKWq\ih9rupwS6/iPjqHQBJ5d6Io120V6KaSvGhbARcyhMkMH+wp9qG7Ica1CFVqZz51X\Wyaq/tvuR71tCXRtaryvQ8J4LCtYssoYYEklfU5sTX1CFRtN15bBr/KLH/1lhZu6\InEOImltDxJ7xNXh1g/EI3RZfLjmXnIfREq+ype4CaI04KIBNthpA72i0WTb/vQI\5WSC1UV/AgMBAAECggEARUO+IMDQkt+NKWGyQq72zVaHNKGHOcanGrniPbVrEG79\Bplc5ZMh0KXGIerMLLCcbPddYnLOklTos1G7fzVERf3nP7AK96nRTJzWHnsHq5xz\/7RY24nHmf4QEFdPuhQD93AcQuTFoXdbZbXLXYajV12OjuMN0VSQdIIdvLVhhWlv\ZLlvUICsFMN4czkSgE22B4hW6pkFVQk+tidI9cI1TtFdBDlLfVOR7+qw/Ve+qqK6\zzKTlKWYVx0TBQvtwylUPC4tKoHNLo3lMeoh5kmkFeBz7R7o3cu4gy62lEO5vdz4\8J8oJ260ravtL0TbIY6uworWZy41ECePp0+h0blngQKBgQDR1Q4eILv67e9VFv9z\rR2BH2ZGvHPPBl+MD/LkrtaEuwNy+VjxsDO6vtAhT3As6s+7n1JL6E4aTZmhRXGk\Vc8Dl4k38pi2O3ciIRzbYcMFij+JYYm0Qseb3x7Iv04r9kFAU2j10qwNNeOjDDcG\t1LPvaqYqy6qmLMBvBpFFcxP4QKBgQDCeLz0PKe2bnXspAB7WUBioxu0BSe4uFxr\QA3QflTdUBSqn8nmOmjej8YpeVg6vqKW7rs50QgJcywVEYV4BROHhNCLYJsOtSqv\m5Syj7yNjBN/RZs1yYRfcdEuY6GmlFNt5y8EGjWNix1Ji1kFpb7+pOW/fq/U+eI3\Bmm0GSvBXwKBgBCC6GJ8hu4+7NdQQPe0Rp8TfnPQfnhq8vfNhXpzO5QkNyhD8LjL\+bYXL79/Rb9zFreX2Nz6QbMWKiGjmkapLeoFcZnCcDvewAgifOfScIsuDsPbtf9G\RfjA/OYlD5yr+wR5y8eUNU+wzuHUozvXDyAjt5nd1oU8ENHxIEwRZAthAoGAY1md\ZsUqBShPdHKgkGObYgjkGUbc8SC2jlAt/orbviiwNi7lzYmfk7wtx3hnm7NSivsx\iSsCCRnetnC6GAO34271v46+CHiDcy1vfP2znTinqUidL5Bg4QXbkPBzYA+8w5Ps\0BK3szUT5EOdWiY/+gWyHe+R0qNKb0QGcmy9js8CgYAnUL3zFfz2cBso1qjez/Hm\zIhPzJzrg+d0Ll81jfQdRtCb9eZnfmCrxZxdXqYGHUyd3GmyNBaQbA8GRZ8usv3Y\IcWf6BQBO1AJqMD+abIKfX8PfQyt41Sq6g3ia2JmasJThnYbEKXh4Jc3ENGsl9HV\cxOPnHqORl4dqV9TFTY5NA==\-----END PRIVATE KEY-----",
                  "passphrase": {
                    "ciphertext": "ZjVmNQ==",
                    "protected": "eyJhbGciOiJkaXIiLCJlbmMiOiJub25lIn0"
                  }
                }
              }
            }
          }
        }

 #. In many environments it is required to perform TLS (SSL) from beginning to end of the communication path.  In this example we will perform SSL Bridging by adding a ServerSSL Profile:

 The body of the post will be as follows:

    .. code-block:: json
        :linenos:

        {
        "class": "AS3",
        "action": "deploy",
        "persist": true,
        "declaration": {
            "class": "ADC",
            "schemaVersion": "3.0.0",
            "id": "123abc",
            "label": "Sample 3",
            "remark": "HTTPS with sslbridging",
            "Sample_03": {
            "class": "Tenant",
            "A1": {
                "class": "Application",
                "template": "https",
                "serviceMain": {
                "class": "Service_HTTPS",
                "virtualAddresses": [
                    "10.1.10.46"
                ],
                "pool": "web_pool",
                "clientTLS": {
                    "bigip": "/Common/serverssl"
                },
                "serverTLS": "webtls"
                },
                "web_pool": {
                "class": "Pool",
                "loadBalancingMode": "predictive-node",
                "monitors": [
                "https"
                ],
                "members": [{
                    "servicePort": 443,
                    "shareNodes": true,
                    "serverAddresses": [
                    "10.1.10.31",
                    "10.1.10.32"
                    ]
                }]
                },
                "webtls": {
                "class": "TLS_Server",
                "certificates": [{
                    "certificate": "webcert"
                }]
                },
                "webcert": {
                "class": "Certificate",
                "remark": "in practice we recommend using a passphrase",
                "certificate": "-----BEGIN CERTIFICATE-----\MIIC8jCCAdqgAwIBAgIJAKQOdKoHQWwwMA0GCSqGSIb3DQEBCwUAMDExLzAtBgNV\BAMMJmlwLTEwLTEtMS00LnVzLXdlc3QtMi5jb21wdXRlLmludGVybmFsMB4XDTE3\MDMyNzE0MzY1MVoXDTI3MDMyNTE0MzY1MVowMTEvMC0GA1UEAwwmaXAtMTAtMS0x\LTQudXMtd2VzdC0yLmNvbXB1dGUuaW50ZXJuYWwwggEiMA0GCSqGSIb3DQEBAQUA\A4IBDwAwggEKAoIBAQCfZmtyGhTH2nXpzczMwOPLQx6lcoqla/h2F/TZd+hSEtra\I+GSAYpqrrT4w03u9pI3u3xpW4DUCYUijyLw9WfkjEgb9/gommkXA5oyrLAJPgFe\KHUTrK21gsc6Aw6X4ewArxMjcgtPkKWqih9rupwS6/iPjqHQBJ5d6Io120V6KaSv\GhbARcyhMkMH+wp9qG7Ica1CFVqZz51XWyaq/tvuR71tCXRtaryvQ8J4LCtYssoY\YEklfU5sTX1CFRtN15bBr/KLH/1lhZu6InEOImltDxJ7xNXh1g/EI3RZfLjmXnIf\REq+ype4CaI04KIBNthpA72i0WTb/vQI5WSC1UV/AgMBAAGjDTALMAkGA1UdEwQC\MAAwDQYJKoZIhvcNAQELBQADggEBADX+1PFBVJu260HbshB/yJV6NUL+m4+S1ux1\EwEve39UHW3z/UUJu9V3Gli9DxF/5J5CY7TPl9w+Vj5/budrm4A45p5aoXDU4J3e\ZYcYcAJmvwGzFleuMImO3cLOIoAvGt2ztQ6yo9xNICutz/fmxZbhwBK08dSluShS\B/3lqfQgBEIjtXqTepbcycPmxx0adnEVD5ObDUKPE0nvm8wSg17+E/DNHdKsbbcR\3uqbq19PSafrZ5YQfzkCqJb7weLc9O5tfagunDwFVDsArbvgTrHJ6io8o2HhdjS8\Sem0nZlzapnhxzqa0jz9fRKJhYidyHt6B+/Vj8Owocvc023CIrY=\-----END CERTIFICATE-----",
                "privateKey": "-----BEGIN PRIVATE KEY-----\MIIEvAIBADANBgkqhkiG9w0BAQEFAASCBKYwggSiAgEAAoIBAQCfZmtyGhTH2nXp\zczMwOPLQx6lcoqla/h2F/TZd+hSEtraI+GSAYpqrrT4w03u9pI3u3xpW4DUCYUi\jyLw9WfkjEgb9/gommkXA5oyrLAJPgFeKHUTrK21gsc6Aw6X4ewArxMjcgtPkKWq\ih9rupwS6/iPjqHQBJ5d6Io120V6KaSvGhbARcyhMkMH+wp9qG7Ica1CFVqZz51X\Wyaq/tvuR71tCXRtaryvQ8J4LCtYssoYYEklfU5sTX1CFRtN15bBr/KLH/1lhZu6\InEOImltDxJ7xNXh1g/EI3RZfLjmXnIfREq+ype4CaI04KIBNthpA72i0WTb/vQI\5WSC1UV/AgMBAAECggEARUO+IMDQkt+NKWGyQq72zVaHNKGHOcanGrniPbVrEG79\Bplc5ZMh0KXGIerMLLCcbPddYnLOklTos1G7fzVERf3nP7AK96nRTJzWHnsHq5xz\/7RY24nHmf4QEFdPuhQD93AcQuTFoXdbZbXLXYajV12OjuMN0VSQdIIdvLVhhWlv\ZLlvUICsFMN4czkSgE22B4hW6pkFVQk+tidI9cI1TtFdBDlLfVOR7+qw/Ve+qqK6\zzKTlKWYVx0TBQvtwylUPC4tKoHNLo3lMeoh5kmkFeBz7R7o3cu4gy62lEO5vdz4\8J8oJ260ravtL0TbIY6uworWZy41ECePp0+h0blngQKBgQDR1Q4eILv67e9VFv9z\rR2BH2ZGvHPPBl+MD/LkrtaEuwNy+VjxsDO6vtAhT3As6s+7n1JL6E4aTZmhRXGk\Vc8Dl4k38pi2O3ciIRzbYcMFij+JYYm0Qseb3x7Iv04r9kFAU2j10qwNNeOjDDcG\t1LPvaqYqy6qmLMBvBpFFcxP4QKBgQDCeLz0PKe2bnXspAB7WUBioxu0BSe4uFxr\QA3QflTdUBSqn8nmOmjej8YpeVg6vqKW7rs50QgJcywVEYV4BROHhNCLYJsOtSqv\m5Syj7yNjBN/RZs1yYRfcdEuY6GmlFNt5y8EGjWNix1Ji1kFpb7+pOW/fq/U+eI3\Bmm0GSvBXwKBgBCC6GJ8hu4+7NdQQPe0Rp8TfnPQfnhq8vfNhXpzO5QkNyhD8LjL\+bYXL79/Rb9zFreX2Nz6QbMWKiGjmkapLeoFcZnCcDvewAgifOfScIsuDsPbtf9G\RfjA/OYlD5yr+wR5y8eUNU+wzuHUozvXDyAjt5nd1oU8ENHxIEwRZAthAoGAY1md\ZsUqBShPdHKgkGObYgjkGUbc8SC2jlAt/orbviiwNi7lzYmfk7wtx3hnm7NSivsx\iSsCCRnetnC6GAO34271v46+CHiDcy1vfP2znTinqUidL5Bg4QXbkPBzYA+8w5Ps\0BK3szUT5EOdWiY/+gWyHe+R0qNKb0QGcmy9js8CgYAnUL3zFfz2cBso1qjez/Hm\zIhPzJzrg+d0Ll81jfQdRtCb9eZnfmCrxZxdXqYGHUyd3GmyNBaQbA8GRZ8usv3Y\IcWf6BQBO1AJqMD+abIKfX8PfQyt41Sq6g3ia2JmasJThnYbEKXh4Jc3ENGsl9HV\cxOPnHqORl4dqV9TFTY5NA==\-----END PRIVATE KEY-----",
                "passphrase": {
                    "ciphertext": "ZjVmNQ==",
                    "protected": "eyJhbGciOiJkaXIiLCJlbmMiOiJub25lIn0"
                }
                }
            }
            }
        }
        }