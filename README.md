# binCraft-decoder
- A decoder library for readsb *.binCraft files for python
- Supporting decompressing of ZSTD compressed files (aircraft.binCraft.zst)
- Decoder returns aircraft.json like dict

## Prepare
```
pip3 install -r requirements.txt
```

## Example
### From File
```python
import binCraft_decoder
data = binCraft_decoder.binCraftReader('aircraft.binCraft.zstd', zstd_compressed=True)
print(data)
```
### From Anonymous Object
```python
import binCraft_decoder
data = binCraft_decoder.binCraftReader(obj, zstd_compressed=True)
print(data)
```

## Advanced Example
### Get aircraft.binCraft.zst from tar1090 then Output JSON
```python
import binCraft_decoder
import http.client
import json

url = '127.0.0.1'
port = 8080
path = '/data/aircraft.binCraft.zst'

conn = http.client.HTTPConnection(url, port)
conn.request('GET', path)

response = conn.getresponse()
raw = response.read()

conn.close()

data = binCraft_decoder.binCraftReader(raw, True)

# | jq . is Good...
print(json.dumps(data, ensure_ascii=False))

output_json_filename = 'example'
dir = f'/path/to/{output_json_filename}.json'
with open(dir, mode="wt", encoding="utf-8") as f:
    json.dump(data, f, ensure_ascii=False)
``` 
