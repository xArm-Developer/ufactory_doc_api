
# 1.xarm\_move\_step

192.168.1.66:18333/ws

按钮： [实时控制] - X+ / X- / Y+ / Y- / Z+ / Z-

长按：保持向某个方向移动，直到按钮被释放，需要与 xarm_move_step_over 和 xarm_move_step_online 配合使用。

短按：向某个方向移动一步。


### **Message** 

```applescript

{
    "cmd": "xarm_move_step",
    "data": {
        "isLoop": true,
        "direction": "joint-angle-increase",
        "isMoveTool": true,
        "acc": 500,
        "axis": 3
    },
    "id": "1"
}
```



### Request：

   

|  data    | 数据类型  |  必须    |           描述           | 示例值 |
| ---------- | ------------- | ----------|------------------------ |----------|
| isloop | bool | 是 | 是否长按移动 | Ture |
| direction | string | 是 | 位置控制:<br/>'position-x-increase’， 'position-x-decrease’， ’position-y-increase’，’position-y-decrease’，’position-z-increase’，’position-z-decrease’，’attitude-roll-increase’，'attitude-roll-decrease'，’attitude-pitch-increase’，'attitude-pitch-decrease'，'attitude-yaw-increase'，'attitude-yaw-decrease'；<br/>关节控制：<br/>'joint-angle-increase'，'joint-angle-decrease';<br/>末端调平：<br/>'set-end-level'. | 'position-x-increase’ |
| isMoveTool | bool | 是 | 是否是移动末端的运动 | True |
| isSetInitialPoint | bool | 否 | （前端已停用）是否设置为初始点位为基本点运动，不传默认为否，direction为'joint-angle-*’的时候使用 | True                  |

### Respons:

```php
code=0->成功；
code!=0->失败，详情参考xArm_Python_SDK/xarm_api_code.md文档。
```
### 代码示例
**background**

```php
if direction == 'position-x-increase' or direction == 'position-x-decrease':
    self._xarm_sync_tcp(0)
    x = GLOBAL.XArm.xarm_position_step if direction == 'position-x-increase' else -GLOBAL.XArm.xarm_position_step
    if isMoveTool:
        code = GLOBAL.XArm.xarm.set_tool_position(x=x, speed=tcp_speed)
    else:
        code = GLOBAL.XArm.xarm.set_position(x=x, relative=True, speed=tcp_speed)
```

**front_end**

```php
moveStep(direction, isLoop) {
      window.CommandsRobotSocket.moveStep({ 'direction': direction, 'isLoop': isLoop, 'isMoveTool': this.isToolCoord });
},

moveStep = (data, callback) => {
  const params = window.GlobalConstant.INIT_CMD_PARAMS_COMMON_DATA();
  Object.assign(params.data, data);
  Object.assign(params.data, {
    mode: self.model.robot.state.info.xarm_mode,
  });
  self.sendCmd(window.GlobalConstant.MOVE_STEP_START, params, (dict) => {
    if (callback) {
      callback(dict);
    }
  });
}
```
