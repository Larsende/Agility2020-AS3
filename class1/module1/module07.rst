Lab 6 - Creating a Applications using AS3 Declarative Interface & BIG-IQ
--------------------------------------------------------------------------------------------------
In this lab, we will create a simple HTTP application using AS3.

Unless instructed, throughout this section we will be working in the ``BIG-IQ`` Collection in Postman. This folder holds 15 enclosed requests that we will work through.

If the error ``Cannot find any ADC root nodes for the target devices`` occurs, follow directions here: https://clouddocs.f5.com/products/extensions/f5-appsvcs-extension/latest/userguide/troubleshooting.html




**Exercise 1 - Authenticate to the BIG-IQ**

#. Open the ``Authenticate to BIG-IQ`` request. Note that tokens need to be refreshed after 5 minutes. 401 Errors will occur if the token is bad or old. Resending this request will refresh the token.

#. Click on the ``Body`` section of the POST Request. The body will look like the following

    .. code-block:: json
        :linenos:

        {
            "username": "admin",
            "password": "admin",
            "loginProviderName": "tmos"
        }

#. Click the blue ``Send`` button. Ensure that you receive a 200 OK response.









**Exercise 2 - Deploy and Delete an App without an App Template on BIG-IQ**

#. Open the ``Deploy App w/o Template`` request.

#. Click on the ``Body`` section of the POST Request. The body will look like the following

    .. code-block:: json
        :linenos:

        {
            "class": "AS3",
            "declaration": {
                "class": "ADC",
                "schemaVersion": "3.7.0",
                "id": "thisisanid",
                "label": "App1",
                "remark": "HTTP with custom persistence",
                "target": {
                    "hostname": "bigip01.as3lab.com"
                },
                "DemoService": {
                    "class": "Tenant",
                    "A1": {
                        "class": "Application",
                        "template": "http",
                        "serviceMain": {
                            "class": "Service_HTTP",
                            "virtualAddresses": [
                                "10.1.10.10"
                            ],
                            "pool": "web_pool"
                        },
                        "web_pool": {
                            "class": "Pool",
                            "monitors": [
                                "http"
                            ],
                            "members": [{
                            "servicePort": 8080,
                            "serverAddresses": [
                                "192.168.128.10"
                            ]
                            }]
                        }
                    }
                }
            }
        }
        
#. Looking further into this request, the URI is sent to the IP address of BIG-IQ instead of the BIG-IP address per past requests. Lines 9-11 target the BIG-IP we would like to build this application on. Lines 12-40 build an http application we have in past sections in this lab.

#. Click the blue ``Send`` button. Ensure that you receive a 200 OK response. 

#. This application is now in the ``Demo Service`` partition on the BIG-IP (10.1.1.4).

#. Navigate to Chrome, BIG-IQ bookmark 10.1.1.7 (username = admin, password = admin).

#. On BIG-IQ, navigate to ``Applications``, ``Applications`` screen to view the deployed application.

    .. image:: /_static/bigiq_1.jpg

#. Now we will move our applciations from ``Unknown Applications`` to another tile named ``Known Applications``.

#. Send the ``Get Application Reference`` request to set the variable ``_bigiq_app_ref``.  Look at the ``Tests`` window for the declaration to see the variable being set, whcih will be used in the following step.

   .. image:: /_static/big_variable.JPG

#. Open the ``Move out of Unknown App`` request.

#. Click the blue ``Send`` button. Ensure that you receive a 200 OK response. 

#. Navigate back to the BIG-IQ Applications and notice that our app is now under the ``Known Applications`` tile.

#. Now that we have had some fun, lets delete the app. Open the ``Delete App w/o Template`` request. 

#. Click the blue ``Send`` button. Ensure that you receive a 200 OK response. 

#. This application is now deleted from BIG-IQ and BIG-IP (10.1.1.4).




**Exercise 3 - Deploy, Change and Delete Apps via App Templates on BIG-IQ**

#. Open the ``Upload App Template to BIG-IQ`` request. Note that this this request was taken from https://github.com/f5devcentral/f5-big-iq.

#. Click on the ``Body`` section of the POST Request. The body will look like the following

    .. code-block:: json
        :linenos:

        {
            "description": "For load balancing an HTTP application on port 80.",
            "name": "AS3-F5-HTTP-lb-template-big-iq-default-v1",
            "published": true,
            "isUICompatible": true,
            "tenant": {
                "name": "",
                "override": false,
                "editable": true
            },
            "schemaOverlay": {
                "type": "object",
                "properties": {
                    "class": {
                        "type": "string",
                        "const": "Application"
                    },
                    "template": {},
                    "schemaOverlay": {},
                    "label": {},
                    "remark": {}
                },
                "additionalProperties": {
                    "allOf": [
                        {
                            "anyOf": [
                                {
                                    "properties": {
                                        "class": {
                                            "const": "Analytics_Profile"
                                        }
                                    }
                                },
                                {
                                    "properties": {
                                        "class": {
                                            "const": "HTTP_Profile"
                                        }
                                    }
                                },
                                {
                                    "properties": {
                                        "class": {
                                            "const": "Pool"
                                        }
                                    }
                                },
                                {
                                    "properties": {
                                        "class": {
                                            "const": "Service_HTTP"
                                        }
                                    }
                                }
                            ]
                        },
                        {
                            "if": {
                                "properties": {
                                    "class": {
                                        "const": "Analytics_Profile"
                                    }
                                }
                            },
                            "then": {
                                "$ref": "#/definitions/Analytics_Profile"
                            }
                        },
                        {
                            "if": {
                                "properties": {
                                    "class": {
                                        "const": "HTTP_Profile"
                                    }
                                }
                            },
                            "then": {
                                "$ref": "#/definitions/HTTP_Profile"
                            }
                        },
                        {
                            "if": {
                                "properties": {
                                    "class": {
                                        "const": "Pool"
                                    }
                                }
                            },
                            "then": {
                                "$ref": "#/definitions/Pool"
                            }
                        },
                        {
                            "if": {
                                "properties": {
                                    "class": {
                                        "const": "Service_HTTP"
                                    }
                                }
                            },
                            "then": {
                                "$ref": "#/definitions/Service_HTTP"
                            }
                        }
                    ]
                },
                "required": [
                    "class"
                ],
                "definitions": {
                    "Analytics_Profile": {
                        "properties": {
                            "class": {},
                            "collectUserAgent": {
                                "type": "boolean"
                            },
                            "collectClientSideStatistics": {
                                "type": "boolean",
                                "default": true
                            },
                            "collectGeo": {
                                "type": "boolean"
                            },
                            "collectUrl": {
                                "type": "boolean"
                            },
                            "collectPageLoadTime": {
                                "type": "boolean"
                            },
                            "collectOsAndBrowser": {
                                "type": "boolean",
                                "default": false
                            },
                            "collectMethod": {
                                "type": "boolean",
                                "default": false
                            },
                            "collectResponseCode": {
                                "type": "boolean",
                                "default": true
                            },
                            "collectIp": {
                                "type": "boolean"
                            }
                        },
                        "type": "object",
                        "additionalproperties": false
                    },
                    "HTTP_Profile": {
                        "properties": {
                            "class": {},
                            "fallbackRedirect": {
                                "type": "string",
                                "default": "https://www.example.com/404"
                            },
                            "fallbackStatusCodes": {
                                "type": "array",
                                "default": [
                                    404
                                ]
                            }
                        },
                        "type": "object",
                        "additionalproperties": false
                    },
                    "Pool": {
                        "properties": {
                            "class": {},
                            "members": {
                                "type": "array",
                                "items": {
                                    "type": "object",
                                    "properties": {
                                        "servicePort": {
                                            "type": "number",
                                            "default": 80
                                        },
                                        "monitors": {
                                            "type": "array",
                                            "default": [
                                                "http"
                                            ],
                                            "const": [
                                                "http"
                                            ]
                                        },
                                        "adminState": {
                                            "type": "string",
                                            "default": "enable"
                                        },
                                        "shareNodes": {
                                            "type": "boolean",
                                            "default": true,
                                            "const": true
                                        },
                                        "serverAddresses": {
                                            "type": "array"
                                        }
                                    }
                                }
                            },
                            "monitors": {
                                "type": "array",
                                "default": [
                                    "http"
                                ],
                                "const": [
                                    "http"
                                ]
                            }
                        },
                        "type": "object",
                        "additionalproperties": false
                    },
                    "Service_HTTP": {
                        "properties": {
                            "class": {},
                            "virtualPort": {
                                "type": "number",
                                "default": 80
                            },
                            "profileAnalytics": {
                                "type": "object",
                                "properties": {
                                    "use": {
                                        "type": "string",
                                        "default": "Analytics_Profile"
                                    }
                                }
                            },
                            "profileHTTP": {
                                "type": "object",
                                "properties": {
                                    "use": {
                                        "type": "string",
                                        "default": "HTTP_Profile"
                                    }
                                }
                            },
                            "virtualAddresses": {
                                "type": "array"
                            },
                            "pool": {
                                "type": "string",
                                "default": "Pool"
                            },
                            "enable": {
                                "type": "boolean",
                                "default": true
                            }
                        },
                        "type": "object",
                        "additionalproperties": false
                    }
                }
            }
        }

#. Looking further into this request Lines 3 names the application template. Line 4 marks the application template as published. Lines 11-257 defines the schema for the application template.

#. Click the blue ``Send`` button. Ensure that you receive a 200 OK response. 

#. To view the template we just uploaded, navigate to Chrome, BIG-IQ bookmark 10.1.1.7 (username = admin, password = admin).

#. On BIG-IQ, navigate to ``Applications``, ``Application Templates``.

    .. image:: /_static/bigiq_2.jpg

#. Open the ``Create App2 with Template`` request.

#. Click on the ``Body`` section of the POST Request. The body will look like the following

    .. code-block:: json
        :linenos:

        {
            "class": "AS3",
            "declaration": {
                "class": "ADC",
                "target": {
                    "hostname": "bigip01.as3lab.com"
                },
                "schemaVersion": "3.7.0",
                "DemoService": {
                    "class": "Tenant",
                    "App2": {
                        "class": "Application",
                        "schemaOverlay": "AS3-F5-HTTP-lb-template-big-iq-default-v1",
                        "template": "http",
                        "serviceMain": {
                            "class": "Service_HTTP",
                            "virtualAddresses": ["10.0.2.23"],
                            "pool": "web_pool"
                        },
                        "web_pool": {
                            "class": "Pool"
                        }
                    }
                }
            }
        }

#. Looking further into this request. Line 13 defines the schema that we are going to use ``AS3-F5-HTTP-lb-template-big-iq-default-v1``.

#. Click the blue ``Send`` button. Ensure that you receive a 200 OK response. 

#. This application is now in the ``Demo Service`` partition on the BIG-IP (10.1.1.4).

#. Now we will change this application. Open the ``Change App2`` request.

#. Click on the ``Body`` section of the POST Request. Notice the changed IP address

    .. code-block:: json
        :linenos:
        
        {
            "class": "AS3",
            "action": "patch",
            "patchBody": [
                {
                    "class": "ADC",
                    "target": {
                        "address": "10.1.1.4"
                    },
                    "op": "replace",
                    "path": "/DemoService/App2",
                    "value": {
                        "class": "Application",
                        "schemaOverlay": "AS3-F5-HTTP-lb-template-big-iq-default-v1",
                        "template": "http",
                        "serviceMain": {
                            "class": "Service_HTTP",
                            "virtualAddresses": ["10.0.2.24"],
                            "pool": "web_pool"
                        },
                        "web_pool": {
                            "class": "Pool"
                        }
                    }
                }
            ]
        }

#. Click the blue ``Send`` button. Ensure that you receive a 200 OK response. 

#. This application is now changed in the ``Demo Service`` partition on the BIG-IP (10.1.1.4).

#. Now we will delete the app. Open the ``Delete App from Template`` request. 

#. Click the blue ``Send`` button. Ensure that you receive a 200 OK response. 

#. This application is now deleted from BIG-IQ and BIG-IP (10.1.1.4).

#. Finally, delete the application template from the BIG-IQ. Open the ``GET HTTP Application Template`` request and click the blue ``Send`` button. Copy the ``id`` from the Body of the response.

    .. image:: /_static/bigiq_3.jpg

#. Paste the ``id`` to the URL of request ``DELETE HTTP Application Template``.

#. Click the blue ``Send`` button. Ensure that you receive a 200 OK response. 

#. Navigate to Chrome, BIG-IQ bookmark 10.1.1.7 (username = admin: password = admin), Applications, Application Templates. The template is now deleted from the available templates.





