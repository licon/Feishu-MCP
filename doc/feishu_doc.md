# 飞书文档 API 接口文档

## 0. 获取登录token

### 请求
```bash
curl -i -X POST 'https://open.feishu.cn/open-apis/auth/v3/app_access_token/internal' \
-H 'Content-Type: application/json' \
-d '{
    "app_id": "<your_app_id>",
    "app_secret": "<your_app_secret>"
}'
```

### 返回结果
```json
{
  "app_access_token": "<access_token>",
  "code": 0,
  "expire": 6055,
  "msg": "ok",
  "tenant_access_token": "<tenant_access_token>"
}
```

## 1. 创建飞书文档

### 请求
```bash
curl -i -X POST 'https://open.feishu.cn/open-apis/docx/v1/documents' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer <access_token>' \
-d '{
    "folder_token": "<folder_token>",
    "title": "一篇新的文档"
}'
```

### 返回结果
```json
{
  "code": 0,
  "data": {
    "document": {
      "document_id": "<document_id>",
      "revision_id": 1,
      "title": "一篇新的文档"
    }
  },
  "msg": "success"
}
```

## 2. 获取文档基本信息

### 请求
```bash
curl -i -X GET 'https://open.feishu.cn/open-apis/docx/v1/documents/<document_id>' \
-H 'Authorization: Bearer <access_token>'
```

### 返回结果
```json
{
  "code": 0,
  "data": {
    "document": {
      "display_setting": {
        "show_authors": true,
        "show_comment_count": false,
        "show_create_time": false,
        "show_like_count": false,
        "show_pv": false,
        "show_related_matters": false,
        "show_uv": false
      },
      "document_id": "<document_id>",
      "revision_id": 1,
      "title": "一篇新的文档"
    }
  },
  "msg": "success"
}
```

## 3. 获取文档中的纯文本内容

### 请求
```bash
curl -i -X GET 'https://open.feishu.cn/open-apis/docx/v1/documents/<document_id>/raw_content?lang=0' \
-H 'Authorization: Bearer <access_token>'
```

### 返回结果
```json
{
  "code": 0,
  "data": {
    "content": "哈哈哈=1\n一级标题\n二级标题\n功能一\n功能二\n第一点\n第二点\n代码块-kotlin\n\n"
  },
  "msg": "success"
}
```

## 4. 获取文档中的块

### 请求
```bash
curl -i -X GET 'https://open.feishu.cn/open-apis/docx/v1/documents/<document_id>/blocks?document_revision_id=-1&page_size=500' \
-H 'Authorization: Bearer <access_token>'
```

### 返回结果
```json
{
  "code": 0,
  "data": {
    "has_more": false,
    "items": [
      {
        "block_id": "<block_id>",
        "block_type": 1,
        "children": ["<child_block_id>"],
        "page": {
          "elements": [
            {
              "text_run": {
                "content": "示例文本",
                "text_element_style": {
                  "bold": false,
                  "inline_code": false,
                  "italic": false,
                  "strikethrough": false,
                  "underline": false
                }
              }
            }
          ],
          "style": {
            "align": 1
          }
        },
        "parent_id": ""
      }
    ]
  },
  "msg": "success"
}
```

## 5. 创建块

### 请求
```bash
curl -i -X POST 'https://open.feishu.cn/open-apis/docx/v1/documents/<document_id>/blocks/<block_id>/children?document_revision_id=-1' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer <access_token>' \
-d '{
    "children": [
        {
            "block_type": 2,
            "text": {
                "elements": [
                    {
                        "text_run": {
                            "content": "多人实时协同，插入一切元素。不仅是在线文档，更是",
                            "text_element_style": {
                                "bold": false,
                                "inline_code": false,
                                "italic": false,
                                "strikethrough": false,
                                "text_color": 5,
                                "underline": false
                            }
                        }
                    }
                ],
                "style": {
                    "align": 1,
                    "folded": false
                }
            }
        }
    ],
    "index": 0
}'
```

## 6. 添加代码块

### 请求
```bash
curl -i -X POST 'https://open.feishu.cn/open-apis/docx/v1/documents/<document_id>/blocks/<block_id>/children?document_revision_id=-1' \
-H 'Content-Type: application/json' \
-d '{
    "children": [
        {
            "block_type": 14,
            "code": {
                "elements": [
                    {
                        "text_run": {
                            "content": "hello world",
                            "text_element_style": {
                                "bold": false,
                                "inline_code": false,
                                "italic": false,
                                "strikethrough": false,
                                "underline": false
                            }
                        }
                    }
                ],
                "style": {
                    "language": 32,
                    "wrap": false
                }
            }
        }
    ],
    "index": 3
}'
```

> 注：代码块语言类型对照表：
> 1: PlainText, 2: ABAP, 3: Ada, 4: Apache, 5: Apex, 6: Assembly Language, 7: Bash, 8: CSharp, 9: C++, 10: C, 11: COBOL, 12: CSS, 13: CoffeeScript, 14: D, 15: Dart, 16: Delphi, 17: Django, 18: Dockerfile, 19: Erlang, 20: Fortran, 22: Go, 23: Groovy, 24: HTML, 25: HTMLBars, 26: HTTP, 27: Haskell, 28: JSON, 29: Java, 30: JavaScript, 31: Julia, 32: Kotlin, 33: LateX, 34: Lisp, 36: Lua, 37: MATLAB, 38: Makefile, 39: Markdown, 40: Nginx, 41: Objective-C, 43: PHP, 44: Perl, 46: Power Shell, 47: Prolog, 48: ProtoBuf, 49: Python, 50: R, 52: Ruby, 53: Rust, 54: SAS, 55: SCSS, 56: SQL, 57: Scala, 58: Scheme, 60: Shell, 61: Swift, 62: Thrift, 63: TypeScript, 64: VBScript, 65: Visual Basic, 66: XML, 67: YAML, 68: CMake, 69: Diff, 70: Gherkin, 71: GraphQL, 72: OpenGL Shading Language, 73: Properties, 74: Solidity, 75: TOML

## 7. 更新块文本内容
* 请求
```
curl -i -X PATCH 'https://open.feishu.cn/open-apis/docx/v1/documents/Jg28dOoZ0ofnMdxcfDrcwJE6n0b/blocks/RXZTdBl6qoL5wzxSyuRcMHFnn4d?document_revision_id=-1' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer t-g1043vjC5MA5A6P2TONFYSFS553GLB3YGNGH3E66' \
-d '{
	"update_text_elements": {
		"elements": [
			{
				"text_run": {
					"content": "测试",
					"text_element_style": {
						"background_color": 2,
						"bold": true,
						"italic": true,
						"strikethrough": true,
						"text_color": 2,
						"underline": true
					}
				}
			},
			{
				"text_run": {
					"content": "文本",
					"text_element_style": {
						"italic": true
					}
				}
			}
		]
	}
}'
```
* 返回数据：
```
{
  "code": 0,
  "data": {
    "block": {
      "block_id": "SJi5dloCxoVRiWxq6fgcd89znyf",
      "block_type": 14,
      "code": {
        "elements": [
          {
            "text_run": {
              "content": "测试",
              "text_element_style": {
                "background_color": 2,
                "bold": true,
                "inline_code": false,
                "italic": true,
                "strikethrough": true,
                "text_color": 2,
                "underline": true
              }
            }
          },
          {
            "text_run": {
              "content": "文本",
              "text_element_style": {
                "bold": false,
                "inline_code": false,
                "italic": true,
                "strikethrough": false,
                "underline": false
              }
            }
          }
        ],
        "style": {
          "wrap": true
        }
      },
      "parent_id": "Jg28dOoZ0ofnMdxcfDrcwJE6n0b"
    },
    "client_token": "881fafb3-3ff4-4181-900d-0ebb7f104627",
    "document_revision_id": 20
  },
  "msg": "success"
}
```

## 8.获取块内容
* 请求
```
curl -i -X GET 'https://open.feishu.cn/open-apis/docx/v1/documents/Jg28dOoZ0ofnMdxcfDrcwJE6n0b/blocks/doxcn4e6moAlWwQL4eevxgQDAIh?document_revision_id=-1' \
-H 'Authorization: Bearer t-g1043vjC5MA5A6P2TONFYSFS553GLB3YGNGH3E66'
```
* 返回结果
```
{
  "code": 0,
  "data": {
    "block": {
      "block_id": "doxcn4e6moAlWwQL4eevxgQDAIh",
      "block_type": 4,
      "heading2": {
        "elements": [
          {
            "text_run": {
              "content": "测试",
              "text_element_style": {
                "background_color": 2,
                "bold": true,
                "inline_code": false,
                "italic": true,
                "strikethrough": true,
                "text_color": 2,
                "underline": true
              }
            }
          },
          {
            "text_run": {
              "content": "文本",
              "text_element_style": {
                "bold": false,
                "inline_code": false,
                "italic": true,
                "strikethrough": false,
                "underline": false
              }
            }
          }
        ],
        "style": {
          "align": 1,
          "folded": false
        }
      },
      "parent_id": "Jg28dOoZ0ofnMdxcfDrcwJE6n0b"
    }
  },
  "msg": "success"
}
```

## 9. 创建无序列表块
* 请求参数：
```
curl -i -X POST 'https://open.feishu.cn/open-apis/docx/v1/documents/Jg28dOoZ0ofnMdxcfDrcwJE6n0b/blocks/Jg28dOoZ0ofnMdxcfDrcwJE6n0b/children?document_revision_id=-1' \
-H 'Authorization: Bearer t-g1043vjC5MA5A6P2TONFYSFS553GLB3YGNGH3E66' \
-H 'Content-Type: application/json' \
-d '{
	"children": [
		{
			"block_type": 12,
			"bullet": {
				"elements": [
					{
						"text_run": {
							"content": "无序列表二",
							"text_element_style": {
								"bold": false,
								"inline_code": false,
								"italic": false,
								"strikethrough": false,
								"underline": false
							}
						}
					}
				],
				"style": {
					"align": 1,
					"folded": false
				}
			}
		}
	],
	"index": 0
}'
```
* 返回数据
```
{
  "code": 0,
  "data": {
    "children": [
      {
        "block_id": "doxcnogvtkjGJwdAH9ZNbI7mCoh",
        "block_type": 12,
        "bullet": {
          "elements": [
            {
              "text_run": {
                "content": "无序列表二",
                "text_element_style": {
                  "bold": false,
                  "inline_code": false,
                  "italic": false,
                  "strikethrough": false,
                  "underline": false
                }
              }
            }
          ],
          "style": {
            "align": 1,
            "folded": false
          }
        },
        "parent_id": "Jg28dOoZ0ofnMdxcfDrcwJE6n0b"
      }
    ],
    "client_token": "27625ea1-8c77-4bca-9d2f-f6a5c3d9fcb6",
    "document_revision_id": 67
  },
  "msg": "success"
}
```

## 10.创建有无列表块
* 请求参数
```
curl -i -X POST 'https://open.feishu.cn/open-apis/docx/v1/documents/Jg28dOoZ0ofnMdxcfDrcwJE6n0b/blocks/Jg28dOoZ0ofnMdxcfDrcwJE6n0b/children?document_revision_id=-1' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer t-g1043vjC5MA5A6P2TONFYSFS553GLB3YGNGH3E66' \
-d '{
	"children": [
		{
			"block_type": 13,
			"ordered": {
				"elements": [
					{
						"text_run": {
							"content": "无序列表二",
							"text_element_style": {
								"bold": false,
								"inline_code": false,
								"italic": false,
								"strikethrough": false,
								"underline": false
							}
						}
					}
				],
				"style": {
					"align": 1,
					"folded": false
				}
			}
		}
	],
	"index": 0
}'
```
* 返回数据
```
{
  "code": 0,
  "data": {
    "children": [
      {
        "block_id": "doxcn920u7BwSlQ4yzFxcZO72fb",
        "block_type": 13,
        "ordered": {
          "elements": [
            {
              "text_run": {
                "content": "无序列表二",
                "text_element_style": {
                  "bold": false,
                  "inline_code": false,
                  "italic": false,
                  "strikethrough": false,
                  "underline": false
                }
              }
            }
          ],
          "style": {
            "align": 1,
            "folded": false
          }
        },
        "parent_id": "Jg28dOoZ0ofnMdxcfDrcwJE6n0b"
      }
    ],
    "client_token": "2f164140-822e-4324-9306-0725a5e69bff",
    "document_revision_id": 68
  },
  "msg": "success"
}
```

## 11.把wiki文档id转成documentId(只有转成documentId才能进行后续操作)
* 请求接口：
```
curl -i -X GET 'https://open.feishu.cn/open-apis/wiki/v2/spaces/get_node?obj_type=wiki&token=PdDWwIHD6iV8jFkInMhcIOY7npg' \
-H 'Authorization: Bearer u-cgqN.PmJ90f87V9xSThOkSl42THl4lRbX2001gA22DDc'
```
* 返回数据
```
{
  "code": 0,
  "data": {
    "node": {
      "creator": "ou_14a35ea3607bb853af3a84b589161b82",
      "has_child": true,
      "node_create_time": "1741868733",
      "node_creator": "ou_14a35ea3607bb853af3a84b589161b82",
      "node_token": "PdDWwIHD6iV8jFkInMhcIOY7npg",
      "node_type": "origin",
      "obj_create_time": "1741868733",
      "obj_edit_time": "1741868733",
      "obj_token": "YLidd9mlKovX6dxJZmicGBBMnQg",
      "obj_type": "docx",
      "origin_node_token": "PdDWwIHD6iV8jFkInMhcIOY7npg",
      "origin_space_id": "7481269240683888644",
      "owner": "ou_14a35ea3607bb853af3a84b589161b82",
      "parent_node_token": "",
      "space_id": "7481269240683888644",
      "title": "首页"
    }
  },
  "msg": "success"
}
```

## 12. 删除块
* 请求接口：
```
curl -i -X DELETE 'https://open.feishu.cn/open-apis/docx/v1/documents/WJsOd6selovWbBxU5Ptc5DfBnOe/blocks/WJsOd6selovWbBxU5Ptc5DfBnOe/children/batch_delete?document_revision_id=-1' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer t-g1044gk6B5HSWOVH4B26WXAPRUY66HAF36KRR25E' \
-d '{
	"end_index": 1,
	"start_index": 0
}'
```
* 返回数据：
```
{
  "code": 0,
  "data": {
    "client_token": "9271d423-208e-4f8c-a86f-75ff8e1efb82",
    "document_revision_id": 3
  },
  "msg": "success"
}
```

### 13. 获取图片资源
* 请求接口：
```
curl -i -X GET 'https://open.feishu.cn/open-apis/drive/v1/medias/boxcnrHpsg1QDqXAAAyachabcef/download?extra=%E6%97%A0' \
-H 'Authorization: Bearer t-7f1b******8e560'
```
* 返回数据
返回文件二进制流

### 14.获取根文件夹信息
* 请求接口：
```
curl --location 'https://open.feishu.cn/open-apis/drive/explorer/v2/root_folder/meta' \
--header 'Authorization: Bearer t-e13d5ec1954e82e458f3ce04491c54ea8c9abcef'
```
* 返回数据：
```
{
  "code": 0,
  "msg": "Success",
  "data": {
    "token": "nodbcbHUdOsS613xVzTzFEabcef",
    "id": "7110173013420512356",
    "user_id": "7103496998321312356"
	}
}
```

### 15. 获取文件夹中的文件清单
* 请求接口：
```
curl -i -X GET 'https://open.feishu.cn/open-apis/drive/v1/files?direction=DESC&folder_token=C4xYfOKM5ldYzod41TUcmOLFnR6&order_by=EditedTime' \
-H 'Authorization: Bearer t-g1044ihxDPBYG2UZWJXWUTVKXGK4OFDG62LW6TXZ'
```
* 返回数据：
```
{
  "code": 0,
  "data": {
    "files": [
      {
        "created_time": "1744972693",
        "modified_time": "1744972693",
        "name": "产品优化项目",
        "owner_id": "ou_c8eb5395f0581cf47190271e032d2e92",
        "parent_token": "C4xYfOKM5ldYzod41TUcmOLFnR6",
        "token": "FWK2fMleClICfodlHHWc4Mygnhb",
        "type": "folder",
        "url": "https://vq5iayk07bc.feishu.cn/drive/folder/FWK2fMleClICfodlHHWc4Mygnhb"
      },
      {
        "created_time": "1744904770",
        "modified_time": "1744904825",
        "name": "Android",
        "owner_id": "ou_14a35ea3607bb853af3a84b589161b82",
        "parent_token": "C4xYfOKM5ldYzod41TUcmOLFnR6",
        "token": "GMgQftudRlaSqbdLimkcxGQWnLb",
        "type": "folder",
        "url": "https://vq5iayk07bc.feishu.cn/drive/folder/GMgQftudRlaSqbdLimkcxGQWnLb"
      },
      {
        "created_time": "1744904794",
        "modified_time": "1744904794",
        "name": "Kotlin",
        "owner_id": "ou_14a35ea3607bb853af3a84b589161b82",
        "parent_token": "C4xYfOKM5ldYzod41TUcmOLFnR6",
        "token": "VYu5fWOYhl0PUFdaPnUcvvn2nvc",
        "type": "folder",
        "url": "https://vq5iayk07bc.feishu.cn/drive/folder/VYu5fWOYhl0PUFdaPnUcvvn2nvc"
      },
      {
        "created_time": "1744973513",
        "modified_time": "1744973518",
        "name": "test",
        "owner_id": "ou_14a35ea3607bb853af3a84b589161b82",
        "parent_token": "C4xYfOKM5ldYzod41TUcmOLFnR6",
        "token": "UEMld79l5oSm2Zxf8NOcFtu7n3e",
        "type": "docx",
        "url": "https://vq5iayk07bc.feishu.cn/docx/UEMld79l5oSm2Zxf8NOcFtu7n3e"
      }
    ],
    "has_more": false
  },
  "msg": "success"
}
```

### 16.新建文件夹
* 请求接口：
```
curl -i -X POST 'https://open.feishu.cn/open-apis/drive/v1/files/create_folder' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer t-g1044ihxDPBYG2UZWJXWUTVKXGK4OFDG62LW6TXZ' \
-d '{
	"folder_token": "C4xYfOKM5ldYzod41TUcmOLFnR6",
	"name": "产品优化项目"
}'
```
* 返回数据：
```
{
  "code": 0,
  "data": {
    "token": "FWK2fMleClICfodlHHWc4Mygnhb",
    "url": "https://vq5iayk07bc.feishu.cn/drive/folder/FWK2fMleClICfodlHHWc4Mygnhb"
  },
  "msg": "success"
}
```