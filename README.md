# ClusterKubernetes

![Diagram](https://github.com/DiegoJimenez14/ClusterKubernetes/assets/115049957/05d6a7e6-ec58-4dec-acf9-14c946788f7c)
## Paramteros 
"Parameters": {
        "VpcBlock": {
            "Type": "String",
            "Default": "192.168.0.0/16",
            "Description": "The CIDR range for the VPC. This should be a valid private (RFC 1918) CIDR range."
        },
        "Subnet01Block": {
            "Type": "String",
            "Default": "192.168.64.0/18",
            "Description": "CidrBlock for subnet 01 within the VPC"
        },
        "Subnet02Block": {
            "Type": "String",
            "Default": "192.168.128.0/18",
            "Description": "CidrBlock for subnet 02 within the VPC"
        },
        "Subnet03Block": {
            "Type": "String",
            "Default": "192.168.192.0/18",
            "Description": "CidrBlock for subnet 03 within the VPC. This is used only if the region has more than 2 AZs."
        }
    }
}
## Condiciones 
{
    "Conditions": {
        "Has2Azs": {
            "Fn::Or": [
                {
                    "Fn::Equals": [
                        {
                            "Ref": "AWS::Region"
                        },
                        "ap-south-1"
                    ]
                },
                {
                    "Fn::Equals": [
                        {
                            "Ref": "AWS::Region"
                        },
                        "ap-northeast-2"
                    ]
                },
                {
                    "Fn::Equals": [
                        {
                            "Ref": "AWS::Region"
                        },
                        "ca-central-1"
                    ]
                },
                {
                    "Fn::Equals": [
                        {
                            "Ref": "AWS::Region"
                        },
                        "cn-north-1"
                    ]
                },
                {
                    "Fn::Equals": [
                        {
                            "Ref": "AWS::Region"
                        },
                        "sa-east-1"
                    ]
                },
                {
                    "Fn::Equals": [
                        {
                            "Ref": "AWS::Region"
                        },
                        "us-west-1"
                    ]
                }
            ]
        },
        "HasMoreThan2Azs": {
            "Fn::Not": [
                {
                    "Condition": "Has2Azs"
                }
            ]
        }
    }
}

                    "width": 240,
                    "height": 240
                },
                "position": {
                    "x": 70,
                    "y": 110
                },
                "z": 1,
                "embeds": [
                    "af6ba16e-6b35-4cc6-b12d-c54a15c62104"
                ]
            },
            "007c3ca7-1fc4-4255-95af-66995677a8ee": {
                "source": {
                    "id": "52890cc4-6225-4902-ab07-04d27bac69ae"
                },
                "target": {
                    "id": "4426d230-4185-462d-8919-3d1290576606"
                }
            },
            "af6ba16e-6b35-4cc6-b12d-c54a15c62104": {
                "size": {
                    "width": 60,
                    "height": 60
                },
                "position": {
                    "x": 100,
                    "y": 170
                },
                "z": 2,
                "parent": "6a64b959-a1f9-4d81-876d-30468d51c188",
                "embeds": [],
                "isassociatedwith": [
                    "4426d230-4185-462d-8919-3d1290576606"
                ],
                "iscontainedinside": [
                    "6a64b959-a1f9-4d81-876d-30468d51c188"
                ],
                "dependson": [
                    "007c3ca7-1fc4-4255-95af-66995677a8ee"
                ]
            }
        }
    }
}
## Salidas 
{
    "Outputs": {
        "SubnetIds": {
            "Description": "All subnets in the VPC",
            "Value": {
                "Fn::If": [
                    "HasMoreThan2Azs",
                    {
                        "Fn::Join": [
                            ",",
                            [
                                {
                                    "Ref": "Subnet01"
                                },
                                {
                                    "Ref": "Subnet02"
                                },
                                {
                                    "Ref": "Subnet03"
                                }
                            ]
                        ]
                    },
                    {
                        "Fn::Join": [
                            ",",
                            [
                                {
                                    "Ref": "Subnet01"
                                },
                                {
                                    "Ref": "Subnet02"
                                }
                            ]
                        ]
                    }
                ]
            }
        },
        "SecurityGroups": {
            "Description": "Security group for the cluster control plane communication with worker nodes",
            "Value": {
                "Fn::Join": [
                    ",",
                    [
                        {
                            "Ref": "ControlPlaneSecurityGroup"
                        }
                    ]
                ]
            }
        },
        "VpcId": {
            "Description": "The VPC Id",
            "Value": {
                "Ref": "VPC"
            }
        }
    }
}
