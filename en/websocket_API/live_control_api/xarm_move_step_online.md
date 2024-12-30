---
title: xarm_move_step_online
order: 4
---

# xarm\_move\_step\_online

### 1. Introduction

Firmware/Studio: 2.5.0+

For long-press button, it's essential to monitor whether the arms are connected. After sending xarm\_move\_step, it is necessary to send xarm\_move\_step\_online command at least every 0.2 seconds.

### 2. Request & Response

**Message**

````json

{
    "cmd": "xarm_move_step_online",
    "id": "1"
}

````
**Request**

<table data-full-width="true"><thead><tr><th>Name</th><th>Type</th><th>Required fields</th><th>Description</th></tr></thead><tbody><tr><td>/</td><td></td><td></td><td></td></tr></tbody></table>


**Response**

```
code=0->success;
code!=0->Failed, refer to xarm_api_code;
```
