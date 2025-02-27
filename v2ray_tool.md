```
import requests
import base64
import json
import copy


config = {}
data = ""
# 打开并读取 JSON 文件
with open('template.json', 'r', encoding='utf-8') as file:
    content = file.read()
    config = json.loads(content)  # 解析 JSON 数据



outbounds_tmplate = config["outbounds"][0]
inbounds_tmplate = config["inbounds"][0]
rule_tmplate = config['routing']['rules'][0]
print("--------------")
#print(json.dumps(outbounds_tmplate, indent=4))
#print(json.dumps(inbounds_tmplate, indent=4))
print(json.dumps(rule_tmplate, indent=4))
print("--------------")
config['outbounds'].remove(outbounds_tmplate)
config["inbounds"].remove(inbounds_tmplate)
#config['inbounds'].clear()
#config["inbounds"].clear()
#config['routing']['rules'].clear()

# uri="https://mojie.app/api/v1/client/subscribe?token=23b907ae830fd36b70a6d7262b6b0bac"

# # 发送 GET 请求
# response = requests.get(uri)

with open('substring.txt', 'r', encoding='utf-8') as file:
    content = file.read()
    data = content
    #print(data)
    

raw_data = base64.b64decode(data)
vpslink_sets = raw_data.decode('utf-8').splitlines()
port_start = 9090

for link in vpslink_sets:
    # print(link)
    # print()
    vps_info = link.split("://")
    print(vps_info)
    if "trojan" == vps_info[0]:
        pass
    elif "vmess" == vps_info[0]:
        vps_data = base64.b64decode(vps_info[1])
        vps_json = json.loads(vps_data.decode('utf-8'))

        vps_json_copy = copy.deepcopy(vps_json)
        outbounds_tmplate_copy = copy.deepcopy(outbounds_tmplate)
        inbounds_tmplate_copy = copy.deepcopy(inbounds_tmplate)
        rule_tmplate_copy = copy.deepcopy(rule_tmplate)
        print(vps_json_copy)
        #print(vps_json)
        outbounds_tmplate_copy['tag'] = vps_json_copy['ps']
        outbounds_tmplate_copy['settings']['vnext'][0]['address'] = vps_json_copy['add']
        outbounds_tmplate_copy['settings']['vnext'][0]['port'] = int(vps_json_copy['port'])
        outbounds_tmplate_copy['settings']['vnext'][0]['users'][0]['id'] = vps_json_copy['id']
        outbounds_tmplate_copy['settings']['vnext'][0]['users'][0]['alterId'] = int(vps_json_copy['aid'])
        # outbounds_tmplate['settings']['vnext'][0]['users'][0]['email'] = vps_json['port']
        # outbounds_tmplate['settings']['vnext'][0]['users'][0]['security'] = vps_json['port']
        if 'ws' == vps_json['net']:
            outbounds_tmplate_copy['streamSettings']['network'] = vps_json_copy['net']
            outbounds_tmplate_copy['streamSettings']['wsSettings']['path'] = vps_json_copy['path']
            outbounds_tmplate_copy['streamSettings']['wsSettings']['host'] = vps_json_copy['host']
            outbounds_tmplate_copy['streamSettings']['wsSettings']['headers']['Host'] = vps_json_copy['host']
        
        port_start = port_start + 1
        inbounds_tmplate_copy['tag'] = vps_json_copy['ps']
        inbounds_tmplate_copy['port'] = port_start

        rule_tmplate_copy['inboundTag'][0] = vps_json_copy['ps']
        rule_tmplate_copy['outboundTag'] = vps_json_copy['ps']
        
        config['outbounds'].append(outbounds_tmplate_copy)
        config['inbounds'].append(inbounds_tmplate_copy)
        config['routing']['rules'].append(rule_tmplate_copy)
        pass
    else:
        print(json.dumps(config, indent=4))  


# 打开文件（如果文件不存在，会创建文件）
with open('output.json', 'w', encoding='utf-8') as file:
    file.write(json.dumps(config, indent=4))

print("文件写入成功")

#print(config)
# # 打印响应内容
# # print(response.text)
# raw_data  = base64.b64decode(response.text)
# vps_data = raw_data.decode()

# print(vps_data)
# vpslink_sets = vps_data.splitlines()
 

    
```

