# Window：deviceorientation 事件

**`deviceorientation`** 事件在方向传感器输出新数据的时候触发。其数据系传感器与地球坐标系相比较所得，也就是说在设备上可能会采用设备地磁计的数据。

参见[有关方向与运动信息的说明](https://developer.mozilla.org/zh-CN/docs/Web/API/Device_orientation_events/Orientation_and_motion_data_explained)。

该事件不可取消也不会冒泡。

## [语法](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/deviceorientation_event#语法)

在 [`addEventListener()`](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener) 方法中使用事件名称，或使用事件处理器属性。

```js
addEventListener('deviceorientation', event => { });

ondeviceorientation = event => { };
```

## [事件类型](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/deviceorientation_event#事件类型)

一个 [`DeviceOrientationEvent`](https://developer.mozilla.org/zh-CN/docs/Web/API/DeviceOrientationEvent)。继承了 [`Event`](https://developer.mozilla.org/zh-CN/docs/Web/API/Event)。

![image-20230705163139618](https://images-1305186932.cos.ap-beijing.myqcloud.com/images/202307051631240.png)

## [事件属性](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/deviceorientation_event#事件属性)

-   [`DeviceOrientationEvent.absolute`](https://developer.mozilla.org/zh-CN/docs/Web/API/DeviceOrientationEvent/absolute) 只读

    一个布尔值，表示设备是否提供了方向数据。

-   [`DeviceOrientationEvent.alpha`](https://developer.mozilla.org/zh-CN/docs/Web/API/DeviceOrientationEvent/alpha) 只读

    一个数字，表示设备围绕 z 轴的转动度数，范围为 0（含）到 360（不含）。

-   [`DeviceOrientationEvent.beta`](https://developer.mozilla.org/zh-CN/docs/Web/API/DeviceOrientationEvent/beta) 只读

    一个数字，表示设备围绕 x 轴的转动度数，范围为 -180（含）到 180（不含）。表示设备的前后运动。

-   [`DeviceOrientationEvent.gamma`](https://developer.mozilla.org/zh-CN/docs/Web/API/DeviceOrientationEvent/gamma) 只读

    一个数字，表示设备围绕 y 轴的转动度数，范围为 -90（含）到 90（不含）。表示设备的左右运动。

-   `DeviceOrientationEvent.webkitCompassHeading` 非标准 只读

    一个数字，表示设备所得到的世界坐标系的 z 轴与正北方向的角度，范围为 0 到 360。

-   `DeviceOrientationEvent.webkitCompassAccuracy` 非标准 只读

    指南针的精准度，以正/负偏差的形式给出。通常是 10。

## [示例](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/deviceorientation_event#示例)

```js
if (window.DeviceOrientationEvent) {
    window.addEventListener("deviceorientation", function(event) {
        // alpha: rotation around z-axis
        var rotateDegrees = event.alpha;
        // gamma: left to right
        var leftToRight = event.gamma;
        // beta: front back motion
        var frontToBack = event.beta;

        handleOrientationEvent(frontToBack, leftToRight, rotateDegrees);
    }, true);
}

var handleOrientationEvent = function(frontToBack, leftToRight, rotateDegrees) {
    // do something amazing
};
```