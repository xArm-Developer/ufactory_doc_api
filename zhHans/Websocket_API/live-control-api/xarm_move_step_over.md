# 2.xarm_move_step_over

192.168.1.66:18333/ws

停止移动机械臂：
实现按钮松开停止机械臂移动功能，需配合xarm_move_step接口一起使用。

**Message:**
```php
{
    "cmd": "xarm_move_step_over",
    "id":"1"
}
```

**Request：**
* cmd
  * string，必填，请求接口

* id

    * string，必填，请求序列号



**Respons:**

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
