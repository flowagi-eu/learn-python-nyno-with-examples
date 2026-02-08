# Learn to use Powerful [Nyno (AI) Workflows build using a GUI](https://github.com/flowagi-eu/nyno) in Python!

![Nyno examples connecting multiple AI nodes](https://github.com/flowagi-eu/nyno/raw/main/h/c0f8c2c19f52c63ba139a25e5fa5fbc80a36a865c1368534bac204c3fc3d683f/screenshot-from-2026-01-12-13-26-24.webp)


### Run Any .nyno File in Python:
```
from nynoclient import NynoClient
client = NynoClient(
    credentials="change_me",
    host="127.0.0.1",
    port=9024,
)

with open('your-nyno-file.nyno','r') as r:
    content = r.read()

result = client.run_nyno(content)
print(result) # { "status": "OK", "execution": [...] }
```

### Run Any .nyno Files with Custom Context Variables:
```
from nynoclient import NynoClient
client = NynoClient(
    credentials="change_me",
    host="127.0.0.1",
    port=9024,
)

with open('your-nyno-file.nyno','r') as r:
    content = r.read()

result = client.run_nyno(content, {"PROMPT":"Do a SWOT analysis of ..."})
print(result) # { "status": "OK", "execution": [...] }
```

### Use a Workflow as Loop
```
from nynoclient import NynoClient
client = NynoClient(
    credentials="change_me",
    host="127.0.0.1",
    port=9024,
)

with open('your-nyno-file.nyno','r') as r:
    content = r.read()

# 1. Workflow one should set "prev" to a list
result = client.run_nyno(content) # prev should be a list
list_length = len(result.get('execution'))
my_list = result.get('execution')[list_length-1].get('output').get('c').get('prev')

# 2. Loop over the list with new workflow
for i, value in enumerate(my_list):
   print(my_list[i]) # object
```

### Loop & Interwine Multiple Workflows:
```
from nynoclient import NynoClient
client = NynoClient(
    credentials="change_me",
    host="127.0.0.1",
    port=9024,
)

with open('your-nyno-file.nyno','r') as r:
    content = r.read()

# 1. Workflow one should set "prev" to a list
result = client.run_nyno(content) # prev should be a list
list_length = len(result.get('execution'))
my_list = result.get('execution')[list_length-1].get('output').get('c').get('prev')

# 2. Open the workflow for single items
with open('your-single-item-workflow.nyno','r') as r:
    content_for_single_item = r.read()

# 3. Loop over the list with new workflow
for i, value in enumerate(my_list):
   single_item_workflow_result = client.run_nyno(content_for_single_item, {"prev":my_list[i]}) # list item as input

   # 3.1 Extract "prev" or other context variables from the single item workflow
   obj = result.get('execution')[list_length-1].get('output').get('c').get('prev')

   # 3.2. Optionally: Refine Existing List Item (Memory Efficiently)
   my_list[i] = obj 
```
