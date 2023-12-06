{
  "version": "Notebook/1.0",
  "items": [
    {
      "type": 1,
      "content": {
        "json": ">**Author**\r\n>[Bruno Gabrielli](mailto:bruno.gabrielli@microsoft.com)\r\n>\r\n>**Version 1.2**\r\n>2023-07-06\r\n> - Fixed parameters\r\n>\r\n>**Version 1.1**\r\n>2022-02-16\r\n> - Added more info about Allowed, Detected and Matched requests\r\n>\r\n>**Version 1.0**\r\n>2020-10-22\r\n> - Initial version. Inspired by https://github.com/iamrobdavies/MonitoringExamples/tree/master/ApplicationGateway/Dashboard and https://docs.microsoft.com/en-us/archive/blogs/robdavies/monitoring-application-gateway-with-azure-log-analytics"
      },
      "conditionalVisibility": {
        "parameterName": "_",
        "comparison": "isEqualTo",
        "value": "_"
      },
      "name": "text - 20"
    },
    {
      "type": 1,
      "content": {
        "json": "# Azure Application Gateway Insights#"
      },
      "name": "text - 0"
    },
    {
      "type": 9,
      "content": {
        "version": "KqlParameterItem/1.0",
        "crossComponentResources": [
          "{Workspaces}"
        ],
        "parameters": [
          {
            "id": "99cc0623-1cf2-42ee-ae97-0e1321843c9d",
            "version": "KqlParameterItem/1.0",
            "name": "Subscriptions",
            "type": 6,
            "isRequired": true,
            "multiSelect": true,
            "quote": "'",
            "delimiter": ",",
            "query": "resourcecontainers\r\n| where type == \"microsoft.resources/subscriptions\"\r\n| project value = id, label = name, selected = true",
            "crossComponentResources": [
              "value::selected"
            ],
            "typeSettings": {
              "additionalResourceOptions": [],
              "showDefault": false
            },
            "queryType": 1,
            "resourceType": "microsoft.resourcegraph/resources",
            "label": "Subscription(s)"
          },
          {
            "id": "1e2b3246-fe4e-4d39-b265-7a42a4ae46c1",
            "version": "KqlParameterItem/1.0",
            "name": "WSSubs",
            "type": 6,
            "isRequired": true,
            "multiSelect": true,
            "quote": "'",
            "delimiter": ",",
            "query": "resources\r\n| where type == \"microsoft.operationalinsights/workspaces\"\r\n| distinct subscriptionId\r\n| project value = subscriptionId, label = subscriptionId, selected = true",
            "crossComponentResources": [
              "value::selected"
            ],
            "isHiddenWhenLocked": true,
            "typeSettings": {
              "additionalResourceOptions": []
            },
            "timeContextFromParameter": "TimeRange",
            "queryType": 1,
            "resourceType": "microsoft.resourcegraph/resources"
          },
          {
            "id": "e249244e-b556-4ecb-aae0-36843bc5a5a7",
            "version": "KqlParameterItem/1.0",
            "name": "Workspaces",
            "type": 5,
            "isRequired": true,
            "query": "resources\r\n| where type == \"microsoft.operationalinsights/workspaces\"\r\n| project value = id, label = name, selected = true",
            "crossComponentResources": [
              "{WSSubs}"
            ],
            "typeSettings": {
              "additionalResourceOptions": [],
              "showDefault": false
            },
            "timeContextFromParameter": "TimeRange",
            "queryType": 1,
            "resourceType": "microsoft.resourcegraph/resources",
            "label": ""
          },
          {
            "id": "5e8549d6-22aa-47e0-a84b-bf1848cd44cc",
            "version": "KqlParameterItem/1.0",
            "name": "TimeRange",
            "type": 4,
            "isRequired": true,
            "value": {
              "durationMs": 7776000000
            },
            "typeSettings": {
              "selectableValues": [
                {
                  "durationMs": 300000
                },
                {
                  "durationMs": 900000
                },
                {
                  "durationMs": 1800000
                },
                {
                  "durationMs": 3600000
                },
                {
                  "durationMs": 14400000
                },
                {
                  "durationMs": 43200000
                },
                {
                  "durationMs": 86400000
                },
                {
                  "durationMs": 172800000
                },
                {
                  "durationMs": 259200000
                },
                {
                  "durationMs": 604800000
                },
                {
                  "durationMs": 1209600000
                },
                {
                  "durationMs": 2419200000
                },
                {
                  "durationMs": 2592000000
                },
                {
                  "durationMs": 5184000000
                },
                {
                  "durationMs": 7776000000
                }
              ]
            }
          },
          {
            "id": "ce7bc1af-150e-43c2-a3ee-2e31c86ed5c1",
            "version": "KqlParameterItem/1.0",
            "name": "p_waf",
            "label": "Application Gateway Server",
            "type": 2,
            "isRequired": true,
            "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where ResourceType == \"APPLICATIONGATEWAYS\"\r\n| distinct Resource",
            "crossComponentResources": [
              "{Workspaces}"
            ],
            "typeSettings": {
              "additionalResourceOptions": [],
              "showDefault": false
            },
            "timeContext": {
              "durationMs": 0
            },
            "timeContextFromParameter": "TimeRange",
            "queryType": 0,
            "resourceType": "microsoft.operationalinsights/workspaces",
            "value": "AG-WAF-MAIN"
          },
          {
            "id": "d4b63f41-737d-46e6-a441-81bf8d8467b1",
            "version": "KqlParameterItem/1.0",
            "name": "WAF_AzureDiagnostics_NotEnabled",
            "type": 2,
            "isRequired": true,
            "multiSelect": true,
            "quote": "'",
            "delimiter": ",",
            "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where Resource == ('{p_waf}')\r\n| where Category in (\"ApplicationGatewayFirewallLog\", \"ApplicationGatewayAccessLog\", \"ApplicationGatewayPerformanceLog\")\r\n| summarize count() by Resource\r\n| project value = count_, label = count_, selected = true\r\n",
            "crossComponentResources": [
              "{Workspaces}"
            ],
            "isHiddenWhenLocked": true,
            "typeSettings": {
              "additionalResourceOptions": [],
              "showDefault": false
            },
            "timeContext": {
              "durationMs": 0
            },
            "timeContextFromParameter": "TimeRange",
            "queryType": 0,
            "resourceType": "microsoft.operationalinsights/workspaces"
          }
        ],
        "style": "pills",
        "queryType": 0,
        "resourceType": "microsoft.operationalinsights/workspaces"
      },
      "name": "Parameters"
    },
    {
      "type": 1,
      "content": {
        "json": "<br>\r\n### AzureDiagnostics for '{p_waf}' Azure Application Firewall not configured or not reporting to the selected workspace(s).\r\n---\r\n\r\nAzureDiagnosics has not been configured for the selected Application Gateways or is configured to store data in a different workspace.\r\n\r\nPlease select another workspace or check out the detailed documentation on how to configure the Diagnostic logs at [this](https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-diagnostics#diagnostic-logging) link.",
        "style": "warning"
      },
      "conditionalVisibility": {
        "parameterName": "WAF_AzureDiagnostics_NotEnabled",
        "comparison": "isEqualTo",
        "value": "0"
      },
      "name": "WAF_AzureDiagnostics_NotEnabled"
    },
    {
      "type": 12,
      "content": {
        "version": "NotebookGroup/1.0",
        "groupType": "editable",
        "items": [
          {
            "type": 9,
            "content": {
              "version": "KqlParameterItem/1.0",
              "crossComponentResources": [
                "{Workspaces}"
              ],
              "parameters": [
                {
                  "id": "c7d07d8f-dad7-4b21-9166-10fbd8e5c6fa",
                  "version": "KqlParameterItem/1.0",
                  "name": "IsFirewallLogConfigured",
                  "type": 2,
                  "isRequired": true,
                  "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where Resource in ('{p_waf}')\r\n| where Category == \"ApplicationGatewayFirewallLog\"\r\n| summarize count()",
                  "crossComponentResources": [
                    "{Workspaces}"
                  ],
                  "isHiddenWhenLocked": true,
                  "typeSettings": {
                    "additionalResourceOptions": [
                      "value::1"
                    ],
                    "showDefault": false
                  },
                  "timeContext": {
                    "durationMs": 2592000000
                  },
                  "defaultValue": "value::1",
                  "queryType": 0,
                  "resourceType": "microsoft.operationalinsights/workspaces"
                },
                {
                  "id": "09cd6f07-a37e-4cbe-b4bc-1318e393529e",
                  "version": "KqlParameterItem/1.0",
                  "name": "IsAccessLogConfigured",
                  "type": 2,
                  "isRequired": true,
                  "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where Resource in ('{p_waf}')\r\n| where Category == \"ApplicationGatewayAccessLog\"\r\n| summarize count()",
                  "crossComponentResources": [
                    "{Workspaces}"
                  ],
                  "isHiddenWhenLocked": true,
                  "typeSettings": {
                    "additionalResourceOptions": [
                      "value::1"
                    ],
                    "showDefault": false
                  },
                  "timeContext": {
                    "durationMs": 2592000000
                  },
                  "defaultValue": "value::1",
                  "queryType": 0,
                  "resourceType": "microsoft.operationalinsights/workspaces"
                },
                {
                  "id": "bf61516d-c629-4942-add5-75547b89b6cc",
                  "version": "KqlParameterItem/1.0",
                  "name": "IsPerformanceLogConfigured",
                  "type": 2,
                  "isRequired": true,
                  "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where Resource in ('{p_waf}')\r\n| where Category == \"ApplicationGatewayPerformanceLog\"\r\n//| take 1\r\n| summarize count()",
                  "crossComponentResources": [
                    "{Workspaces}"
                  ],
                  "isHiddenWhenLocked": true,
                  "typeSettings": {
                    "additionalResourceOptions": [
                      "value::1"
                    ],
                    "showDefault": false
                  },
                  "timeContext": {
                    "durationMs": 2592000000
                  },
                  "defaultValue": "value::1",
                  "queryType": 0,
                  "resourceType": "microsoft.operationalinsights/workspaces"
                }
              ],
              "style": "pills",
              "queryType": 0,
              "resourceType": "microsoft.operationalinsights/workspaces"
            },
            "name": "parameters - 4"
          },
          {
            "type": 11,
            "content": {
              "version": "LinkItem/1.0",
              "style": "tabs",
              "links": [
                {
                  "id": "889cafa4-bb3c-4581-af2e-14d5b684ce4e",
                  "cellValue": "selectedTab",
                  "linkTarget": "parameter",
                  "linkLabel": "Firewall Log",
                  "subTarget": "ApplicationGatewayFirewallLog",
                  "style": "link"
                },
                {
                  "id": "3a4e0fee-692c-49b7-95fa-3a8354896c4f",
                  "cellValue": "selectedTab",
                  "linkTarget": "parameter",
                  "linkLabel": "Access Log",
                  "subTarget": "ApplicationGatewayAccessLog",
                  "style": "link"
                }
              ]
            },
            "name": "links - 7"
          },
          {
            "type": 12,
            "content": {
              "version": "NotebookGroup/1.0",
              "groupType": "editable",
              "items": [
                {
                  "type": 9,
                  "content": {
                    "version": "KqlParameterItem/1.0",
                    "parameters": [
                      {
                        "id": "9efaf472-1188-4bf3-b7e7-53289da3c703",
                        "version": "KqlParameterItem/1.0",
                        "name": "p_ExcludeByMsg",
                        "label": "Exclude by rule message",
                        "type": 2,
                        "multiSelect": true,
                        "quote": "'",
                        "delimiter": ",",
                        "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where ResourceType == \"APPLICATIONGATEWAYS\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayFirewallLog\"\r\n| distinct Message",
                        "crossComponentResources": [
                          "{Workspaces}"
                        ],
                        "value": null,
                        "typeSettings": {
                          "additionalResourceOptions": [
                            "value::all"
                          ],
                          "showDefault": false
                        },
                        "timeContext": {
                          "durationMs": 0
                        },
                        "timeContextFromParameter": "TimeRange",
                        "queryType": 0,
                        "resourceType": "microsoft.operationalinsights/workspaces"
                      },
                      {
                        "id": "d87ad219-caea-4c86-a727-1f286a18bc76",
                        "version": "KqlParameterItem/1.0",
                        "name": "excludedRuleMessages",
                        "type": 1,
                        "isHiddenWhenLocked": true,
                        "criteriaData": [
                          {
                            "criteriaContext": {
                              "leftOperand": "p_ExcludeByMsg",
                              "operator": "is Empty",
                              "rightValType": "param",
                              "resultValType": "static",
                              "resultVal": "dynamic(['Blank1','Blank2'])"
                            }
                          },
                          {
                            "criteriaContext": {
                              "operator": "Default",
                              "resultValType": "static",
                              "resultVal": "dynamic([{p_ExcludeByMsg}])"
                            }
                          }
                        ],
                        "timeContext": {
                          "durationMs": 0
                        },
                        "timeContextFromParameter": "TimeRange"
                      }
                    ],
                    "style": "pills",
                    "queryType": 0,
                    "resourceType": "microsoft.operationalinsights/workspaces"
                  },
                  "name": "parameters - 9"
                },
                {
                  "type": 3,
                  "content": {
                    "version": "KqlItem/1.0",
                    "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where ResourceType == \"APPLICATIONGATEWAYS\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayFirewallLog\"\r\n| where Message !in ({excludedRuleMessages})\r\n| summarize count() by action_s\r\n",
                    "size": 1,
                    "showAnalytics": true,
                    "title": "Count of requests by Action",
                    "timeContextFromParameter": "TimeRange",
                    "queryType": 0,
                    "resourceType": "microsoft.operationalinsights/workspaces",
                    "crossComponentResources": [
                      "{Workspaces}"
                    ],
                    "visualization": "piechart",
                    "chartSettings": {
                      "seriesLabelSettings": [
                        {
                          "seriesName": "Detected",
                          "color": "orange"
                        },
                        {
                          "seriesName": "Matched",
                          "color": "blue"
                        },
                        {
                          "seriesName": "Blocked",
                          "color": "redBright"
                        },
                        {
                          "seriesName": "Allowed",
                          "color": "green"
                        }
                      ]
                    }
                  },
                  "showPin": true,
                  "name": "Requests_By_Action",
                  "styleSettings": {
                    "showBorder": true
                  }
                },
                {
                  "type": 11,
                  "content": {
                    "version": "LinkItem/1.0",
                    "style": "tabs",
                    "links": [
                      {
                        "id": "f3b7f63e-e2b6-45d1-9130-a3a8395748da",
                        "cellValue": "selectedTab",
                        "linkTarget": "parameter",
                        "linkLabel": "Allowed",
                        "subTarget": "Allowed",
                        "preText": "Allowed",
                        "style": "link"
                      },
                      {
                        "id": "c1b5de6d-a692-4812-b9cc-17f3aa719a6f",
                        "cellValue": "selectedTab",
                        "linkTarget": "parameter",
                        "linkLabel": "Blocked",
                        "subTarget": "Blocked",
                        "preText": "Blocked",
                        "style": "link"
                      },
                      {
                        "id": "f82495d8-f6c9-4fcf-94f4-be8d7bb5a52b",
                        "cellValue": "selectedTab",
                        "linkTarget": "parameter",
                        "linkLabel": "Detected",
                        "subTarget": "Detected",
                        "style": "link"
                      },
                      {
                        "id": "ae6fe58c-f92d-4871-b5aa-bfce1e550062",
                        "cellValue": "selectedTab",
                        "linkTarget": "parameter",
                        "linkLabel": "Matched",
                        "subTarget": "Matched",
                        "preText": "Matched",
                        "style": "link"
                      }
                    ]
                  },
                  "name": "links - 9"
                },
                {
                  "type": 12,
                  "content": {
                    "version": "NotebookGroup/1.0",
                    "groupType": "editable",
                    "items": [
                      {
                        "type": 3,
                        "content": {
                          "version": "KqlItem/1.0",
                          "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where ResourceType == \"APPLICATIONGATEWAYS\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayFirewallLog\"\r\n| where Message !in ({excludedRuleMessages})\r\n| where action_s == \"Allowed\"\r\n| summarize count() by ruleId_s",
                          "size": 1,
                          "showAnalytics": true,
                          "title": "Count of allowed requests by rule Id",
                          "timeContextFromParameter": "TimeRange",
                          "queryType": 0,
                          "resourceType": "microsoft.operationalinsights/workspaces",
                          "crossComponentResources": [
                            "{Workspaces}"
                          ],
                          "visualization": "piechart"
                        },
                        "customWidth": "50",
                        "name": "CountOfAllowedRequestsByRuleId",
                        "styleSettings": {
                          "padding": "10",
                          "showBorder": true
                        }
                      },
                      {
                        "type": 3,
                        "content": {
                          "version": "KqlItem/1.0",
                          "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where ResourceType == \"APPLICATIONGATEWAYS\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayFirewallLog\"\r\n| where Message !in ({excludedRuleMessages})\r\n| where action_s == \"Allowed\"\r\n| summarize count() by Message\r\n| top 50 by count_ desc",
                          "size": 0,
                          "showAnalytics": true,
                          "title": "Top 50 allowed requests by rule name ",
                          "noDataMessage": "No allowed requests matching the specified criteria",
                          "noDataMessageStyle": 3,
                          "timeContextFromParameter": "TimeRange",
                          "queryType": 0,
                          "resourceType": "microsoft.operationalinsights/workspaces",
                          "crossComponentResources": [
                            "{Workspaces}"
                          ],
                          "gridSettings": {
                            "filter": true,
                            "labelSettings": [
                              {
                                "columnId": "count_",
                                "label": "Count"
                              }
                            ]
                          }
                        },
                        "showPin": true,
                        "name": "Top50_AllowedRequestsByRuleName",
                        "styleSettings": {
                          "showBorder": true
                        }
                      },
                      {
                        "type": 3,
                        "content": {
                          "version": "KqlItem/1.0",
                          "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where ResourceType == \"APPLICATIONGATEWAYS\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayFirewallLog\"\r\n| where Message !in ({excludedRuleMessages})\r\n| where action_s == \"Allowed\"\r\n| summarize count() by ruleSetType_s, ruleSetVersion_s, ruleId_s\r\n| sort by ruleSetType_s asc, ruleSetVersion_s asc, ruleId_s asc",
                          "size": 0,
                          "showAnalytics": true,
                          "title": "Allowed requests by Rule set type, Rule set version and Rule Id",
                          "noDataMessage": "No allowed requests matching the specified criteria",
                          "noDataMessageStyle": 3,
                          "timeContextFromParameter": "TimeRange",
                          "exportedParameters": [
                            {
                              "fieldName": "ruleSetType_s",
                              "parameterName": "p_ruleSetType_s",
                              "parameterType": 1
                            },
                            {
                              "fieldName": "ruleSetVersion_s",
                              "parameterName": "p_ruleSetVersion_s",
                              "parameterType": 1
                            },
                            {
                              "fieldName": "ruleId_s",
                              "parameterName": "p_ruleId_s",
                              "parameterType": 1
                            }
                          ],
                          "queryType": 0,
                          "resourceType": "microsoft.operationalinsights/workspaces",
                          "crossComponentResources": [
                            "{Workspaces}"
                          ],
                          "gridSettings": {
                            "formatters": [
                              {
                                "columnMatch": "ruleSetType_s",
                                "formatter": 5
                              }
                            ],
                            "filter": true,
                            "hierarchySettings": {
                              "treeType": 1,
                              "groupBy": [
                                "ruleSetType_s"
                              ],
                              "expandTopLevel": true
                            },
                            "labelSettings": [
                              {
                                "columnId": "ruleSetType_s",
                                "label": "Rule set type"
                              },
                              {
                                "columnId": "ruleSetVersion_s",
                                "label": "Rule set version"
                              },
                              {
                                "columnId": "ruleId_s",
                                "label": "Rule Id"
                              },
                              {
                                "columnId": "count_",
                                "label": "Count"
                              }
                            ]
                          }
                        },
                        "name": "AllowedRequestByRuleSetType_sAndRuleSetVersion_sAndRuleId_s",
                        "styleSettings": {
                          "showBorder": true
                        }
                      },
                      {
                        "type": 1,
                        "content": {
                          "json": "💡 _Click on the row to select an object to see more details._"
                        },
                        "conditionalVisibility": {
                          "parameterName": "p_requestType",
                          "comparison": "isEqualTo"
                        },
                        "name": "GuideToRequestSelection"
                      },
                      {
                        "type": 3,
                        "content": {
                          "version": "KqlItem/1.0",
                          "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where ResourceType == \"APPLICATIONGATEWAYS\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayFirewallLog\"\r\n| where Message !in ({excludedRuleMessages})\r\n| where action_s == \"Allowed\"\r\n| where ruleSetType_s == (\"{p_ruleSetType_s}\")\r\n| where ruleSetVersion_s == (\"{p_ruleSetVersion_s}\")\r\n| where ruleId_s == ('{p_ruleId_s}')\r\n| project TimeGenerated, timeStamp_t, clientIp_s, hostname_s, ruleId_s, ruleSetType_s, ruleSetVersion_s, Message",
                          "size": 0,
                          "showAnalytics": true,
                          "title": "Details for requests with Rule Set Type == '{p_ruleSetType_s}' , Rule Set Version == '{p_ruleSetVersion_s}' and Rule Id == '{p_ruleId_s}'.",
                          "noDataMessage": "No allowed requests matching the specified criteria",
                          "noDataMessageStyle": 3,
                          "timeContextFromParameter": "TimeRange",
                          "queryType": 0,
                          "resourceType": "microsoft.operationalinsights/workspaces",
                          "crossComponentResources": [
                            "{Workspaces}"
                          ],
                          "gridSettings": {
                            "formatters": [
                              {
                                "columnMatch": "TimeGenerated",
                                "formatter": 0,
                                "formatOptions": {
                                  "customColumnWidthSetting": "31.4286ch"
                                }
                              },
                              {
                                "columnMatch": "timeStamp_t",
                                "formatter": 0,
                                "formatOptions": {
                                  "customColumnWidthSetting": "28.8571ch"
                                }
                              },
                              {
                                "columnMatch": "clientIp_s",
                                "formatter": 0,
                                "formatOptions": {
                                  "customColumnWidthSetting": "18.4286ch"
                                }
                              },
                              {
                                "columnMatch": "hostname_s",
                                "formatter": 0,
                                "formatOptions": {
                                  "customColumnWidthSetting": "45.1429ch"
                                }
                              },
                              {
                                "columnMatch": "Message",
                                "formatter": 5,
                                "formatOptions": {
                                  "customColumnWidthSetting": "50ch"
                                }
                              },
                              {
                                "columnMatch": "ruleId_s",
                                "formatter": 7,
                                "formatOptions": {
                                  "linkTarget": "GenericDetails",
                                  "linkIsContextBlade": true,
                                  "customColumnWidthSetting": "26ch"
                                }
                              },
                              {
                                "columnMatch": "ruleSetType_s",
                                "formatter": 0,
                                "formatOptions": {
                                  "customColumnWidthSetting": "18.7143ch"
                                }
                              },
                              {
                                "columnMatch": "ruleSetVersion_s",
                                "formatter": 5,
                                "formatOptions": {
                                  "customColumnWidthSetting": "22ch"
                                }
                              }
                            ],
                            "filter": true,
                            "labelSettings": [
                              {
                                "columnId": "TimeGenerated",
                                "label": "Time Generated"
                              },
                              {
                                "columnId": "timeStamp_t",
                                "label": "Original TimesStamp"
                              },
                              {
                                "columnId": "clientIp_s",
                                "label": "Client IP"
                              },
                              {
                                "columnId": "hostname_s",
                                "label": "Hostname"
                              },
                              {
                                "columnId": "ruleId_s",
                                "label": "Rule Id"
                              },
                              {
                                "columnId": "ruleSetType_s",
                                "label": "Rule set type"
                              },
                              {
                                "columnId": "ruleSetVersion_s",
                                "label": "Rule set version"
                              },
                              {
                                "columnId": "Message",
                                "label": "Message"
                              }
                            ]
                          }
                        },
                        "conditionalVisibility": {
                          "parameterName": "p_ruleSetType_s",
                          "comparison": "isNotEqualTo"
                        },
                        "name": "AllowedRequestByRuleSetType_sAndRuleSetVersion_sAndRuleId_s_Details"
                      }
                    ]
                  },
                  "conditionalVisibility": {
                    "parameterName": "selectedTab",
                    "comparison": "isEqualTo",
                    "value": "Allowed"
                  },
                  "name": "grp_Allowed"
                },
                {
                  "type": 12,
                  "content": {
                    "version": "NotebookGroup/1.0",
                    "groupType": "editable",
                    "items": [
                      {
                        "type": 3,
                        "content": {
                          "version": "KqlItem/1.0",
                          "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where ResourceType == \"APPLICATIONGATEWAYS\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayFirewallLog\"\r\n| where Message !in ({excludedRuleMessages})\r\n| where action_s == \"Blocked\"\r\n| summarize count() by ruleId_s",
                          "size": 1,
                          "showAnalytics": true,
                          "title": "Count of blocked requests by rule Id",
                          "noDataMessage": "No blocked requests matching the specified criteria",
                          "noDataMessageStyle": 3,
                          "timeContextFromParameter": "TimeRange",
                          "queryType": 0,
                          "resourceType": "microsoft.operationalinsights/workspaces",
                          "crossComponentResources": [
                            "{Workspaces}"
                          ],
                          "visualization": "piechart"
                        },
                        "customWidth": "50",
                        "showPin": true,
                        "name": "CountOfBlockedRequestsByRuleId",
                        "styleSettings": {
                          "showBorder": true
                        }
                      },
                      {
                        "type": 3,
                        "content": {
                          "version": "KqlItem/1.0",
                          "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where ResourceType == \"APPLICATIONGATEWAYS\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayFirewallLog\"\r\n| where Message !in ({excludedRuleMessages})\r\n| where action_s == \"Blocked\"\r\n| summarize count() by Message\r\n| top 50 by count_ desc",
                          "size": 0,
                          "showAnalytics": true,
                          "title": "Top 50 blocked requests by rule name ",
                          "noDataMessage": "No blocked requests matching the specified criteria",
                          "noDataMessageStyle": 3,
                          "timeContextFromParameter": "TimeRange",
                          "queryType": 0,
                          "resourceType": "microsoft.operationalinsights/workspaces",
                          "crossComponentResources": [
                            "{Workspaces}"
                          ],
                          "gridSettings": {
                            "filter": true,
                            "labelSettings": [
                              {
                                "columnId": "count_",
                                "label": "Count"
                              }
                            ]
                          }
                        },
                        "showPin": true,
                        "name": "Top50_BlockedRequestsByRuleName",
                        "styleSettings": {
                          "showBorder": true
                        }
                      },
                      {
                        "type": 3,
                        "content": {
                          "version": "KqlItem/1.0",
                          "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where ResourceType == \"APPLICATIONGATEWAYS\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayFirewallLog\"\r\n| where Message !in ({excludedRuleMessages})\r\n| where action_s == \"Blocked\"\r\n| extend patternMatch = replace(@\"Warning. Pattern match \",@\"\",details_message_s)\r\n| extend regExMatch = tostring(split(patternMatch,\" at \")[0])\r\n| extend requestType = tostring(split(split(patternMatch,\" at \")[1],\":\")[0])\r\n| extend object = tostring(split(split(patternMatch,\" at \")[1],\":\")[1])\r\n| summarize count() by requestType, object, details_file_s\r\n| sort by requestType asc, object asc",
                          "size": 0,
                          "showAnalytics": true,
                          "title": "Blocked requests by request type and object",
                          "noDataMessage": "No blocked requests matching the specified criteria",
                          "noDataMessageStyle": 3,
                          "timeContextFromParameter": "TimeRange",
                          "exportedParameters": [
                            {
                              "fieldName": "requestType",
                              "parameterName": "p_requestType",
                              "parameterType": 1
                            },
                            {
                              "fieldName": "object",
                              "parameterName": "p_object",
                              "parameterType": 1
                            }
                          ],
                          "queryType": 0,
                          "resourceType": "microsoft.operationalinsights/workspaces",
                          "crossComponentResources": [
                            "{Workspaces}"
                          ],
                          "gridSettings": {
                            "formatters": [
                              {
                                "columnMatch": "requestType",
                                "formatter": 5
                              }
                            ],
                            "filter": true,
                            "hierarchySettings": {
                              "treeType": 1,
                              "groupBy": [
                                "requestType"
                              ],
                              "expandTopLevel": true
                            },
                            "labelSettings": [
                              {
                                "columnId": "requestType",
                                "label": "Request Type"
                              },
                              {
                                "columnId": "object",
                                "label": "Request Object"
                              },
                              {
                                "columnId": "details_file_s",
                                "label": "Rule file name"
                              },
                              {
                                "columnId": "count_",
                                "label": "Count"
                              }
                            ]
                          }
                        },
                        "name": "BlockedRequestByRequestTypeAndObject",
                        "styleSettings": {
                          "showBorder": true
                        }
                      },
                      {
                        "type": 1,
                        "content": {
                          "json": "💡 _Click on the row to select an object to see more details._"
                        },
                        "conditionalVisibility": {
                          "parameterName": "p_requestType",
                          "comparison": "isEqualTo"
                        },
                        "name": "GuideToRequestSelection"
                      },
                      {
                        "type": 3,
                        "content": {
                          "version": "KqlItem/1.0",
                          "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where ResourceType == \"APPLICATIONGATEWAYS\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayFirewallLog\"\r\n| where Message !in ({excludedRuleMessages})\r\n| where action_s == \"Blocked\"\r\n| extend patternMatch = replace(@\"Warning. Pattern match \",@\"\",details_message_s)\r\n| extend regExMatch = tostring(split(patternMatch,\" at \")[0])\r\n| extend requestType = tostring(split(split(patternMatch,\" at \")[1],\":\")[0])\r\n| extend object = tostring(split(split(patternMatch,\" at \")[1],\":\")[1])\r\n| extend matchedData = tostring(split(details_data_s,\":\")[3])\r\n| where requestType == (\"{p_requestType}\")\r\n| where object == (\"{p_object}\")\r\n| extend matchedData = iif(isempty(matchedData),\"This field was not populated.\",matchedData)\r\n| extend details_data_s = iif(isempty(details_data_s),\"This field was not populated.\",details_data_s)\r\n| project TimeGenerated, timeStamp_t , regExMatch, requestType, object, matchedData, details_data_s, ruleId_s, ruleSetType_s, ruleSetVersion_s, details_file_s, clientIp_s, requestUri_s, details_message_s",
                          "size": 0,
                          "showAnalytics": true,
                          "title": "Details for requests with Request Type == '{p_requestType}' and Request Object == '{p_object}'. Click on the 'Request Object' column for more details.",
                          "noDataMessage": "No blocked requests matching the specified criteria",
                          "noDataMessageStyle": 3,
                          "timeContextFromParameter": "TimeRange",
                          "queryType": 0,
                          "resourceType": "microsoft.operationalinsights/workspaces",
                          "crossComponentResources": [
                            "{Workspaces}"
                          ],
                          "gridSettings": {
                            "formatters": [
                              {
                                "columnMatch": "TimeGenerated",
                                "formatter": 0,
                                "formatOptions": {
                                  "customColumnWidthSetting": "30ch"
                                }
                              },
                              {
                                "columnMatch": "timeStamp_t",
                                "formatter": 0,
                                "formatOptions": {
                                  "customColumnWidthSetting": "26ch"
                                }
                              },
                              {
                                "columnMatch": "regExMatch",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "requestType",
                                "formatter": 0,
                                "formatOptions": {
                                  "customColumnWidthSetting": "18ch"
                                }
                              },
                              {
                                "columnMatch": "object",
                                "formatter": 1,
                                "formatOptions": {
                                  "linkTarget": "GenericDetails",
                                  "linkIsContextBlade": true,
                                  "customColumnWidthSetting": "21ch"
                                }
                              },
                              {
                                "columnMatch": "matchedData",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "details_data_s",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "ruleId_s",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "ruleSetType_s",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "ruleSetVersion_s",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "details_file_s",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "clientIp_s",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "requestUri_s",
                                "formatter": 0,
                                "formatOptions": {
                                  "customColumnWidthSetting": "71.2857ch"
                                }
                              },
                              {
                                "columnMatch": "details_message_s",
                                "formatter": 5
                              }
                            ],
                            "filter": true,
                            "labelSettings": [
                              {
                                "columnId": "TimeGenerated",
                                "label": "Time Generated"
                              },
                              {
                                "columnId": "timeStamp_t",
                                "label": "Original timestamp"
                              },
                              {
                                "columnId": "regExMatch",
                                "label": "RegEx"
                              },
                              {
                                "columnId": "requestType",
                                "label": "Request Type"
                              },
                              {
                                "columnId": "object",
                                "label": "Request Object"
                              },
                              {
                                "columnId": "matchedData",
                                "label": "RegEx Matched Data"
                              },
                              {
                                "columnId": "details_data_s",
                                "label": "Matched Data Message"
                              },
                              {
                                "columnId": "ruleId_s",
                                "label": "Rule Id"
                              },
                              {
                                "columnId": "ruleSetType_s",
                                "label": "Rule set type"
                              },
                              {
                                "columnId": "ruleSetVersion_s",
                                "label": "Rule set version"
                              },
                              {
                                "columnId": "details_file_s",
                                "label": "Rule file name"
                              },
                              {
                                "columnId": "clientIp_s",
                                "label": "Client IP"
                              },
                              {
                                "columnId": "requestUri_s",
                                "label": "Request URI"
                              }
                            ]
                          }
                        },
                        "conditionalVisibility": {
                          "parameterName": "p_requestType",
                          "comparison": "isNotEqualTo"
                        },
                        "name": "BlockedRequestByRequestTypeAndObject_Details"
                      }
                    ]
                  },
                  "conditionalVisibility": {
                    "parameterName": "selectedTab",
                    "comparison": "isEqualTo",
                    "value": "Blocked"
                  },
                  "name": "grp_Blocked"
                },
                {
                  "type": 12,
                  "content": {
                    "version": "NotebookGroup/1.0",
                    "groupType": "editable",
                    "items": [
                      {
                        "type": 3,
                        "content": {
                          "version": "KqlItem/1.0",
                          "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where ResourceType == \"APPLICATIONGATEWAYS\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayFirewallLog\"\r\n| where Message !in ({excludedRuleMessages})\r\n| where action_s == \"Detected\"\r\n| summarize count() by ruleId_s",
                          "size": 1,
                          "showAnalytics": true,
                          "title": "Count of detected requests by rule Id",
                          "noDataMessage": "No detected requests matching the specified criteria",
                          "noDataMessageStyle": 3,
                          "timeContextFromParameter": "TimeRange",
                          "queryType": 0,
                          "resourceType": "microsoft.operationalinsights/workspaces",
                          "crossComponentResources": [
                            "{Workspaces}"
                          ],
                          "visualization": "piechart"
                        },
                        "customWidth": "50",
                        "showPin": true,
                        "name": "CountOfDetectedRequestsByRuleId",
                        "styleSettings": {
                          "showBorder": true
                        }
                      },
                      {
                        "type": 3,
                        "content": {
                          "version": "KqlItem/1.0",
                          "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where ResourceType == \"APPLICATIONGATEWAYS\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayFirewallLog\"\r\n| where Message !in ({excludedRuleMessages})\r\n| where action_s == \"Detected\"\r\n| summarize count() by Message\r\n| top 50 by count_ desc",
                          "size": 0,
                          "showAnalytics": true,
                          "title": "Top 50 detected requests by rule name ",
                          "noDataMessage": "No detected requests matching the specified criteria",
                          "noDataMessageStyle": 3,
                          "timeContextFromParameter": "TimeRange",
                          "queryType": 0,
                          "resourceType": "microsoft.operationalinsights/workspaces",
                          "crossComponentResources": [
                            "{Workspaces}"
                          ],
                          "gridSettings": {
                            "filter": true,
                            "labelSettings": [
                              {
                                "columnId": "count_",
                                "label": "Count"
                              }
                            ]
                          }
                        },
                        "showPin": true,
                        "name": "Top50_detectedRequestsByRuleName",
                        "styleSettings": {
                          "showBorder": true
                        }
                      },
                      {
                        "type": 3,
                        "content": {
                          "version": "KqlItem/1.0",
                          "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where ResourceType == \"APPLICATIONGATEWAYS\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayFirewallLog\"\r\n| where Message !in ({excludedRuleMessages})\r\n| where action_s == \"Detected\"\r\n| extend patternMatch = replace(@\"Warning. Pattern match \",@\"\",details_message_s)\r\n| extend regExMatch = tostring(split(patternMatch,\" at \")[0])\r\n| extend requestType = tostring(split(split(patternMatch,\" at \")[1],\":\")[0])\r\n| extend object = tostring(split(split(patternMatch,\" at \")[1],\":\")[1])\r\n| summarize count() by requestType, object, details_file_s\r\n| sort by requestType asc, object asc",
                          "size": 0,
                          "showAnalytics": true,
                          "title": "Detected requests by request type and object",
                          "noDataMessage": "No detected requests matching the specified criteria",
                          "noDataMessageStyle": 3,
                          "timeContextFromParameter": "TimeRange",
                          "exportedParameters": [
                            {
                              "fieldName": "requestType",
                              "parameterName": "p_requestType",
                              "parameterType": 1
                            },
                            {
                              "fieldName": "object",
                              "parameterName": "p_object",
                              "parameterType": 1
                            }
                          ],
                          "queryType": 0,
                          "resourceType": "microsoft.operationalinsights/workspaces",
                          "crossComponentResources": [
                            "{Workspaces}"
                          ],
                          "gridSettings": {
                            "formatters": [
                              {
                                "columnMatch": "requestType",
                                "formatter": 5
                              }
                            ],
                            "filter": true,
                            "hierarchySettings": {
                              "treeType": 1,
                              "groupBy": [
                                "requestType"
                              ],
                              "expandTopLevel": true
                            },
                            "labelSettings": [
                              {
                                "columnId": "requestType",
                                "label": "Request Type"
                              },
                              {
                                "columnId": "object",
                                "label": "Request Object"
                              },
                              {
                                "columnId": "details_file_s",
                                "label": "Rule file name"
                              },
                              {
                                "columnId": "count_",
                                "label": "Count"
                              }
                            ]
                          }
                        },
                        "name": "DetectedRequestByRequestTypeAndObject",
                        "styleSettings": {
                          "showBorder": true
                        }
                      },
                      {
                        "type": 1,
                        "content": {
                          "json": "💡 _Click on the row to select an object to see more details._"
                        },
                        "conditionalVisibility": {
                          "parameterName": "p_requestType",
                          "comparison": "isEqualTo"
                        },
                        "name": "GuideToRequestSelection"
                      },
                      {
                        "type": 3,
                        "content": {
                          "version": "KqlItem/1.0",
                          "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where ResourceType == \"APPLICATIONGATEWAYS\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayFirewallLog\"\r\n| where Message !in ({excludedRuleMessages})\r\n| where action_s == \"Detected\"\r\n| extend patternMatch = replace(@\"Warning. Pattern match \",@\"\",details_message_s)\r\n| extend regExMatch = tostring(split(patternMatch,\" at \")[0])\r\n| extend requestType = tostring(split(split(patternMatch,\" at \")[1],\":\")[0])\r\n| extend object = tostring(split(split(patternMatch,\" at \")[1],\":\")[1])\r\n| extend matchedData = tostring(split(details_data_s,\":\")[3])\r\n| where requestType == (\"{p_requestType}\")\r\n| where object == (\"{p_object}\")\r\n| extend matchedData = iif(isempty(matchedData),\"This field was not populated.\",matchedData)\r\n| extend details_data_s = iif(isempty(details_data_s),\"This field was not populated.\",details_data_s)\r\n| project TimeGenerated, timeStamp_t , regExMatch, requestType, object, matchedData, details_data_s, ruleId_s, ruleSetType_s, ruleSetVersion_s, details_file_s, clientIp_s, requestUri_s, details_message_s",
                          "size": 0,
                          "showAnalytics": true,
                          "title": "Details for requests with Request Type == '{p_requestType}' and Request Object == '{p_object}'. Click on the 'Request Object' column for more details.",
                          "noDataMessage": "No detected requests matching the specified criteria",
                          "noDataMessageStyle": 3,
                          "timeContextFromParameter": "TimeRange",
                          "queryType": 0,
                          "resourceType": "microsoft.operationalinsights/workspaces",
                          "crossComponentResources": [
                            "{Workspaces}"
                          ],
                          "gridSettings": {
                            "formatters": [
                              {
                                "columnMatch": "TimeGenerated",
                                "formatter": 0,
                                "formatOptions": {
                                  "customColumnWidthSetting": "30ch"
                                }
                              },
                              {
                                "columnMatch": "timeStamp_t",
                                "formatter": 0,
                                "formatOptions": {
                                  "customColumnWidthSetting": "26ch"
                                }
                              },
                              {
                                "columnMatch": "regExMatch",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "requestType",
                                "formatter": 0,
                                "formatOptions": {
                                  "customColumnWidthSetting": "18ch"
                                }
                              },
                              {
                                "columnMatch": "object",
                                "formatter": 1,
                                "formatOptions": {
                                  "linkTarget": "GenericDetails",
                                  "linkIsContextBlade": true,
                                  "customColumnWidthSetting": "31.8571ch"
                                }
                              },
                              {
                                "columnMatch": "matchedData",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "details_data_s",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "ruleId_s",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "ruleSetType_s",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "ruleSetVersion_s",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "details_file_s",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "clientIp_s",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "requestUri_s",
                                "formatter": 0,
                                "formatOptions": {
                                  "customColumnWidthSetting": "70ch"
                                }
                              },
                              {
                                "columnMatch": "details_message_s",
                                "formatter": 5
                              }
                            ],
                            "filter": true,
                            "labelSettings": [
                              {
                                "columnId": "TimeGenerated",
                                "label": "Time Generated"
                              },
                              {
                                "columnId": "timeStamp_t",
                                "label": "Original timestamp"
                              },
                              {
                                "columnId": "regExMatch",
                                "label": "RegEx"
                              },
                              {
                                "columnId": "requestType",
                                "label": "Request Type"
                              },
                              {
                                "columnId": "object",
                                "label": "Request Object"
                              },
                              {
                                "columnId": "matchedData",
                                "label": "RegEx Matched Data"
                              },
                              {
                                "columnId": "details_data_s",
                                "label": "Matched Data Message"
                              },
                              {
                                "columnId": "ruleId_s",
                                "label": "Rule Id"
                              },
                              {
                                "columnId": "ruleSetType_s",
                                "label": "Rule set type"
                              },
                              {
                                "columnId": "ruleSetVersion_s",
                                "label": "Rule set version"
                              },
                              {
                                "columnId": "details_file_s",
                                "label": "Rule file name"
                              },
                              {
                                "columnId": "clientIp_s",
                                "label": "Client IP"
                              },
                              {
                                "columnId": "requestUri_s",
                                "label": "Request URI"
                              }
                            ]
                          }
                        },
                        "conditionalVisibility": {
                          "parameterName": "p_requestType",
                          "comparison": "isNotEqualTo"
                        },
                        "name": "DetectedRequestByRequestTypeAndObject_Details"
                      }
                    ]
                  },
                  "conditionalVisibility": {
                    "parameterName": "selectedTab",
                    "comparison": "isEqualTo",
                    "value": "Detected"
                  },
                  "name": "grp_Detected"
                },
                {
                  "type": 12,
                  "content": {
                    "version": "NotebookGroup/1.0",
                    "groupType": "editable",
                    "items": [
                      {
                        "type": 3,
                        "content": {
                          "version": "KqlItem/1.0",
                          "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where ResourceType == \"APPLICATIONGATEWAYS\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayFirewallLog\"\r\n| where Message !in ({excludedRuleMessages})\r\n| where action_s == \"Matched\"\r\n| summarize count() by ruleId_s",
                          "size": 1,
                          "showAnalytics": true,
                          "title": "Count of matched requests by rule Id",
                          "noDataMessage": "No matched requests matching the specified criteria",
                          "noDataMessageStyle": 3,
                          "timeContextFromParameter": "TimeRange",
                          "queryType": 0,
                          "resourceType": "microsoft.operationalinsights/workspaces",
                          "crossComponentResources": [
                            "{Workspaces}"
                          ],
                          "visualization": "piechart"
                        },
                        "customWidth": "50",
                        "showPin": true,
                        "name": "CountOfMatchedRequestsByRuleId",
                        "styleSettings": {
                          "showBorder": true
                        }
                      },
                      {
                        "type": 3,
                        "content": {
                          "version": "KqlItem/1.0",
                          "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where ResourceType == \"APPLICATIONGATEWAYS\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayFirewallLog\"\r\n| where Message !in ({excludedRuleMessages})\r\n| where action_s == \"Matched\"\r\n| summarize count() by Message\r\n| top 50 by count_ desc",
                          "size": 0,
                          "showAnalytics": true,
                          "title": "Top 50 matched requests by rule name ",
                          "noDataMessage": "No matched requests matching the specified criteria",
                          "noDataMessageStyle": 3,
                          "timeContextFromParameter": "TimeRange",
                          "queryType": 0,
                          "resourceType": "microsoft.operationalinsights/workspaces",
                          "crossComponentResources": [
                            "{Workspaces}"
                          ],
                          "gridSettings": {
                            "filter": true,
                            "labelSettings": [
                              {
                                "columnId": "count_",
                                "label": "Count"
                              }
                            ]
                          }
                        },
                        "showPin": true,
                        "name": "Top50_MatchedRequestsByRuleName",
                        "styleSettings": {
                          "showBorder": true
                        }
                      },
                      {
                        "type": 3,
                        "content": {
                          "version": "KqlItem/1.0",
                          "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where ResourceType == \"APPLICATIONGATEWAYS\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayFirewallLog\"\r\n| where Message !in ({excludedRuleMessages})\r\n| where action_s == \"Matched\"\r\n| extend patternMatch = replace(@\"Warning. Pattern match \",@\"\",details_message_s)\r\n| extend regExMatch = tostring(split(patternMatch,\" at \")[0])\r\n| extend requestType = tostring(split(split(patternMatch,\" at \")[1],\":\")[0])\r\n| extend object = tostring(split(split(patternMatch,\" at \")[1],\":\")[1])\r\n| summarize count() by requestType, object, details_file_s\r\n| sort by requestType asc, object asc",
                          "size": 0,
                          "showAnalytics": true,
                          "title": "Matched requests by request type and object",
                          "noDataMessage": "No matched requests matching the specified criteria",
                          "noDataMessageStyle": 3,
                          "timeContextFromParameter": "TimeRange",
                          "exportedParameters": [
                            {
                              "fieldName": "requestType",
                              "parameterName": "p_requestType",
                              "parameterType": 1
                            },
                            {
                              "fieldName": "object",
                              "parameterName": "p_object",
                              "parameterType": 1
                            }
                          ],
                          "queryType": 0,
                          "resourceType": "microsoft.operationalinsights/workspaces",
                          "crossComponentResources": [
                            "{Workspaces}"
                          ],
                          "gridSettings": {
                            "formatters": [
                              {
                                "columnMatch": "requestType",
                                "formatter": 5
                              }
                            ],
                            "filter": true,
                            "hierarchySettings": {
                              "treeType": 1,
                              "groupBy": [
                                "requestType"
                              ],
                              "expandTopLevel": true
                            },
                            "labelSettings": [
                              {
                                "columnId": "requestType",
                                "label": "Request Type"
                              },
                              {
                                "columnId": "object",
                                "label": "Request Object"
                              },
                              {
                                "columnId": "details_file_s",
                                "label": "Rule file name"
                              },
                              {
                                "columnId": "count_",
                                "label": "Count"
                              }
                            ]
                          }
                        },
                        "name": "MatchedRequestByRequestTypeAndObject",
                        "styleSettings": {
                          "showBorder": true
                        }
                      },
                      {
                        "type": 1,
                        "content": {
                          "json": "💡 _Click on the row to select an object to see more details._"
                        },
                        "conditionalVisibility": {
                          "parameterName": "p_requestType",
                          "comparison": "isEqualTo"
                        },
                        "name": "GuideToRequestSelection"
                      },
                      {
                        "type": 3,
                        "content": {
                          "version": "KqlItem/1.0",
                          "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where ResourceType == \"APPLICATIONGATEWAYS\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayFirewallLog\"\r\n| where Message !in ({excludedRuleMessages})\r\n| where action_s == \"Matched\"\r\n| extend patternMatch = replace(@\"Warning. Pattern match \",@\"\",details_message_s)\r\n| extend regExMatch = tostring(split(patternMatch,\" at \")[0])\r\n| extend requestType = tostring(split(split(patternMatch,\" at \")[1],\":\")[0])\r\n| extend object = tostring(split(split(patternMatch,\" at \")[1],\":\")[1])\r\n| extend matchedData = tostring(split(details_data_s,\":\")[3])\r\n| where requestType == (\"{p_requestType}\")\r\n| where object == (\"{p_object}\")\r\n| extend matchedData = iif(isempty(matchedData),\"This field was not populated.\",matchedData)\r\n| extend details_data_s = iif(isempty(details_data_s),\"This field was not populated.\",details_data_s)\r\n| project TimeGenerated, timeStamp_t , regExMatch, requestType, object, matchedData, details_data_s, ruleId_s, ruleSetType_s, ruleSetVersion_s, details_file_s, clientIp_s, requestUri_s, details_message_s",
                          "size": 0,
                          "showAnalytics": true,
                          "title": "Details for requests with Request Type == '{p_requestType}' and Request Object == '{p_object}'. Click on the 'Request Object' column for more details.",
                          "noDataMessage": "No matched requests matching the specified criteria",
                          "noDataMessageStyle": 3,
                          "timeContextFromParameter": "TimeRange",
                          "queryType": 0,
                          "resourceType": "microsoft.operationalinsights/workspaces",
                          "crossComponentResources": [
                            "{Workspaces}"
                          ],
                          "gridSettings": {
                            "formatters": [
                              {
                                "columnMatch": "TimeGenerated",
                                "formatter": 0,
                                "formatOptions": {
                                  "customColumnWidthSetting": "30ch"
                                }
                              },
                              {
                                "columnMatch": "timeStamp_t",
                                "formatter": 0,
                                "formatOptions": {
                                  "customColumnWidthSetting": "26ch"
                                }
                              },
                              {
                                "columnMatch": "regExMatch",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "requestType",
                                "formatter": 0,
                                "formatOptions": {
                                  "customColumnWidthSetting": "18ch"
                                }
                              },
                              {
                                "columnMatch": "object",
                                "formatter": 1,
                                "formatOptions": {
                                  "linkTarget": "GenericDetails",
                                  "linkIsContextBlade": true,
                                  "customColumnWidthSetting": "21.8571ch"
                                }
                              },
                              {
                                "columnMatch": "matchedData",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "details_data_s",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "ruleId_s",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "ruleSetType_s",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "ruleSetVersion_s",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "details_file_s",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "clientIp_s",
                                "formatter": 5
                              },
                              {
                                "columnMatch": "requestUri_s",
                                "formatter": 0,
                                "formatOptions": {
                                  "customColumnWidthSetting": "69.2857ch"
                                }
                              },
                              {
                                "columnMatch": "details_message_s",
                                "formatter": 5
                              }
                            ],
                            "filter": true,
                            "labelSettings": [
                              {
                                "columnId": "TimeGenerated",
                                "label": "Time Generated"
                              },
                              {
                                "columnId": "timeStamp_t",
                                "label": "Original timestamp"
                              },
                              {
                                "columnId": "regExMatch",
                                "label": "RegEx"
                              },
                              {
                                "columnId": "requestType",
                                "label": "Request Type"
                              },
                              {
                                "columnId": "object",
                                "label": "Request Object"
                              },
                              {
                                "columnId": "matchedData",
                                "label": "RegEx Matched Data"
                              },
                              {
                                "columnId": "details_data_s",
                                "label": "Matched Data Message"
                              },
                              {
                                "columnId": "ruleId_s",
                                "label": "Rule Id"
                              },
                              {
                                "columnId": "ruleSetType_s",
                                "label": "Rule set type"
                              },
                              {
                                "columnId": "ruleSetVersion_s",
                                "label": "Rule set version"
                              },
                              {
                                "columnId": "details_file_s",
                                "label": "Rule file name"
                              },
                              {
                                "columnId": "clientIp_s",
                                "label": "Client IP"
                              },
                              {
                                "columnId": "requestUri_s",
                                "label": "Request URI"
                              }
                            ]
                          }
                        },
                        "conditionalVisibility": {
                          "parameterName": "p_requestType",
                          "comparison": "isNotEqualTo"
                        },
                        "name": "MatchedRequestByRequestTypeAndObject_Details"
                      }
                    ]
                  },
                  "conditionalVisibility": {
                    "parameterName": "selectedTab",
                    "comparison": "isEqualTo",
                    "value": "Matched"
                  },
                  "name": "grp_Matched"
                },
                {
                  "type": 1,
                  "content": {
                    "json": "<br>\r\n### ⚠️ AzureDiagnostics for the selected '{p_waf}' Azure Application Firewall is not configured to log '{selectedTab}' to the selected workspace(s).\r\n---\r\n\r\nPlease select another workspace or check out the detailed documentation on how to configure the Diagnostic logs at [this](https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-diagnostics#diagnostic-logging) link."
                  },
                  "conditionalVisibilities": [
                    {
                      "parameterName": "selectedTab",
                      "comparison": "isEqualTo",
                      "value": "ApplicationGatewayFirewallLog"
                    },
                    {
                      "parameterName": "IsFirewallLogConfigured",
                      "comparison": "isEqualTo",
                      "value": "0"
                    }
                  ],
                  "name": "warningFirewallLog"
                }
              ]
            },
            "conditionalVisibilities": [
              {
                "parameterName": "selectedTab",
                "comparison": "isEqualTo",
                "value": "ApplicationGatewayFirewallLog"
              },
              {
                "parameterName": "IsFirewallLogConfigured",
                "comparison": "isNotEqualTo",
                "value": "0"
              }
            ],
            "name": "grp_FirewallLog"
          },
          {
            "type": 12,
            "content": {
              "version": "NotebookGroup/1.0",
              "groupType": "editable",
              "items": [
                {
                  "type": 9,
                  "content": {
                    "version": "KqlParameterItem/1.0",
                    "crossComponentResources": [
                      "{Workspaces}"
                    ],
                    "parameters": [
                      {
                        "id": "87ab718a-3cdc-4737-9aa9-1505f41961b8",
                        "version": "KqlParameterItem/1.0",
                        "name": "p_api",
                        "label": "URI",
                        "type": 2,
                        "isRequired": true,
                        "multiSelect": true,
                        "quote": "'",
                        "delimiter": ",",
                        "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayAccessLog\"\r\n| distinct requestUri_s",
                        "crossComponentResources": [
                          "{Workspaces}"
                        ],
                        "typeSettings": {
                          "additionalResourceOptions": [
                            "value::all"
                          ],
                          "showDefault": false
                        },
                        "timeContext": {
                          "durationMs": 2419200000
                        },
                        "timeContextFromParameter": "TimeRange",
                        "defaultValue": "value::all",
                        "queryType": 0,
                        "resourceType": "microsoft.operationalinsights/workspaces",
                        "value": [
                          ""
                        ]
                      }
                    ],
                    "style": "pills",
                    "queryType": 0,
                    "resourceType": "microsoft.operationalinsights/workspaces"
                  },
                  "name": "parameters - 1"
                },
                {
                  "type": 3,
                  "content": {
                    "version": "KqlItem/1.0",
                    "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayAccessLog\"\r\n| where requestUri_s in ({p_api})\r\n| summarize count() by requestUri_s, bin(TimeGenerated, 1m)",
                    "size": 0,
                    "aggregation": 5,
                    "title": "Requests/min by URI",
                    "timeContextFromParameter": "TimeRange",
                    "exportFieldName": "requestURI_s",
                    "exportParameterName": "p_requestURI_s",
                    "queryType": 0,
                    "resourceType": "microsoft.operationalinsights/workspaces",
                    "crossComponentResources": [
                      "{Workspaces}"
                    ],
                    "visualization": "timechart"
                  },
                  "showPin": false,
                  "name": "requestsPerMinByUri"
                },
                {
                  "type": 3,
                  "content": {
                    "version": "KqlItem/1.0",
                    "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayAccessLog\"\r\n| where requestUri_s in ({p_api})\r\n| where httpStatus_d >= 400\r\n| summarize count() by httpStatus_d, requestUri_s\r\n| project httpStatus_d, requestUri_s, count_",
                    "size": 0,
                    "title": "Failed Requests by URI",
                    "timeContextFromParameter": "TimeRange",
                    "queryType": 0,
                    "resourceType": "microsoft.operationalinsights/workspaces",
                    "crossComponentResources": [
                      "{Workspaces}"
                    ],
                    "gridSettings": {
                      "filter": true
                    }
                  },
                  "name": "countOfFailedRequestByURI"
                },
                {
                  "type": 3,
                  "content": {
                    "version": "KqlItem/1.0",
                    "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayAccessLog\"\r\n| where requestUri_s in ({p_api})\r\n| where httpStatus_d >= 400\r\n| summarize count() by serverRouted_s, bin(TimeGenerated, 5m)",
                    "size": 0,
                    "aggregation": 5,
                    "title": "Failed requests by protected application(s).",
                    "timeContextFromParameter": "TimeRange",
                    "queryType": 0,
                    "resourceType": "microsoft.operationalinsights/workspaces",
                    "crossComponentResources": [
                      "{Workspaces}"
                    ],
                    "visualization": "timechart"
                  },
                  "name": "FailedRequestsByProtectedApps"
                },
                {
                  "type": 1,
                  "content": {
                    "json": "💡 _Should you get any returned data in the below chart, please refer to [Troubleshooting bad gateway errors in Application Gateway](https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-troubleshooting-502)."
                  },
                  "name": "text - 5"
                },
                {
                  "type": 3,
                  "content": {
                    "version": "KqlItem/1.0",
                    "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayAccessLog\"\r\n| where httpStatus_d == 502\r\n| summarize count() by serverRouted_s, bin(TimeGenerated, 5m)",
                    "size": 0,
                    "aggregation": 5,
                    "title": "HTTP 502 errors by protected Application(s) (every 5 mins).",
                    "timeContextFromParameter": "TimeRange",
                    "queryType": 0,
                    "resourceType": "microsoft.operationalinsights/workspaces",
                    "crossComponentResources": [
                      "{Workspaces}"
                    ],
                    "visualization": "timechart"
                  },
                  "name": "Http502ByProtectedApps"
                },
                {
                  "type": 1,
                  "content": {
                    "json": "<br>\r\n### ⚠️ AzureDiagnostics for the selected '{p_waf}' Azure Application Firewall is not configured to log '{selectedTab}' to the selected workspace(s).\r\n---\r\n\r\nPlease select another workspace or check out the detailed documentation on how to configure the Diagnostic logs at [this](https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-diagnostics#diagnostic-logging) link."
                  },
                  "conditionalVisibilities": [
                    {
                      "parameterName": "selectedTab",
                      "comparison": "isEqualTo",
                      "value": "ApplicationGatewayAccessLog"
                    },
                    {
                      "parameterName": "IsAccessLogConfigured",
                      "comparison": "isEqualTo",
                      "value": "0"
                    }
                  ],
                  "name": "warningAccessLog"
                }
              ]
            },
            "conditionalVisibilities": [
              {
                "parameterName": "selectedTab",
                "comparison": "isEqualTo",
                "value": "ApplicationGatewayAccessLog"
              },
              {
                "parameterName": "IsAccessLogConfigured",
                "comparison": "isNotEqualTo",
                "value": "0"
              }
            ],
            "name": "grp_AccessLog"
          },
          {
            "type": 12,
            "content": {
              "version": "NotebookGroup/1.0",
              "groupType": "editable",
              "items": [
                {
                  "type": 3,
                  "content": {
                    "version": "KqlItem/1.0",
                    "query": "let unHealthyHostCount = AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayPerformanceLog\"\r\n| summarize max(unHealthyHostCount_d) by Resource, bin(TimeGenerated, 1m);\r\nlet HealthyHostCount = AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayPerformanceLog\"\r\n| summarize max(healthyHostCount_d) by Resource, bin(TimeGenerated, 1m);\r\nunHealthyHostCount\r\n| union HealthyHostCount\r\n",
                    "size": 0,
                    "title": "Count of  backend Virtual Machines by status.",
                    "timeContextFromParameter": "TimeRange",
                    "queryType": 0,
                    "resourceType": "microsoft.operationalinsights/workspaces",
                    "crossComponentResources": [
                      "{Workspaces}"
                    ],
                    "visualization": "timechart"
                  },
                  "name": "CountOfVmByStatus"
                },
                {
                  "type": 3,
                  "content": {
                    "version": "KqlItem/1.0",
                    "query": "let allReqs = AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayPerformanceLog\"\r\n| summarize avg(requestCount_d) by bin(TimeGenerated, 1m);\r\nlet failedReqs = AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayPerformanceLog\"\r\n| summarize avg(failedRequestCount_d) by bin(TimeGenerated, 1m);\r\nallReqs\r\n| union failedReqs\r\n",
                    "size": 0,
                    "title": "Avg Requests per min",
                    "timeContextFromParameter": "TimeRange",
                    "queryType": 0,
                    "resourceType": "microsoft.operationalinsights/workspaces",
                    "crossComponentResources": [
                      "{Workspaces}"
                    ],
                    "visualization": "timechart"
                  },
                  "name": "AvgRequestsPerMin"
                },
                {
                  "type": 3,
                  "content": {
                    "version": "KqlItem/1.0",
                    "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayPerformanceLog\"\r\n| summarize avg(throughput_d) by Resource, bin(TimeGenerated, 1m)\r\n| extend ThroughputMb = (avg_throughput_d/1000)/1000\r\n| project Resource, TimeGenerated, ThroughputMb",
                    "size": 0,
                    "title": "Avg throughput per second (Mb)",
                    "timeContextFromParameter": "TimeRange",
                    "queryType": 0,
                    "resourceType": "microsoft.operationalinsights/workspaces",
                    "crossComponentResources": [
                      "{Workspaces}"
                    ],
                    "visualization": "timechart"
                  },
                  "name": "AvgThroughputPerSecMb"
                },
                {
                  "type": 3,
                  "content": {
                    "version": "KqlItem/1.0",
                    "query": "AzureDiagnostics\r\n| where ResourceProvider == \"MICROSOFT.NETWORK\"\r\n| where Resource == ('{p_waf}')\r\n| where Category == \"ApplicationGatewayPerformanceLog\"\r\n| summarize avg(latency_d) by bin(TimeGenerated, 1m)",
                    "size": 0,
                    "title": "Avg Latency (ms)",
                    "timeContextFromParameter": "TimeRange",
                    "queryType": 0,
                    "resourceType": "microsoft.operationalinsights/workspaces",
                    "crossComponentResources": [
                      "{Workspaces}"
                    ],
                    "visualization": "timechart"
                  },
                  "name": "AvgLatencyMs"
                },
                {
                  "type": 1,
                  "content": {
                    "json": "<br>\r\n### ⚠️ AzureDiagnostics for the selected '{p_waf}' Azure Application Firewall is not configured to log '{selectedTab}' to the selected workspace(s).\r\n---\r\n\r\nPlease select another workspace or check out the detailed documentation on how to configure the Diagnostic logs at [this](https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-diagnostics#diagnostic-logging) link."
                  },
                  "conditionalVisibilities": [
                    {
                      "parameterName": "selectedTab",
                      "comparison": "isEqualTo",
                      "value": "ApplicationGatewayPerformanceLog"
                    },
                    {
                      "parameterName": "IsPerformanceLogConfigured",
                      "comparison": "isEqualTo",
                      "value": "0"
                    }
                  ],
                  "name": "warningPerformanceLog"
                }
              ]
            },
            "conditionalVisibilities": [
              {
                "parameterName": "selectedTab",
                "comparison": "isEqualTo",
                "value": "ApplicationGatewayPerformanceLog"
              },
              {
                "parameterName": "IsPerformanceLogConfigured",
                "comparison": "isNotEqualTo",
                "value": "0"
              }
            ],
            "name": "grp_PerformanceLog"
          }
        ]
      },
      "conditionalVisibility": {
        "parameterName": "WAF_AzureDiagnostics_NotEnabled",
        "comparison": "isNotEqualTo",
        "value": "0"
      },
      "name": "Workbook_Content"
    }
  ],
  "fallbackResourceIds": [
    "Azure Monitor"
  ],
  "fromTemplateId": "sentinel-UserWorkbook",
  "$schema": "https://github.com/Microsoft/Application-Insights-Workbooks/blob/master/schema/workbook.json"
}
