# TGZ48

## Calculating position from revolutions and angle

```c++
auto map = packet.asMap();
const uint32_t drive1Angle = static_cast<int32_t>(map[TGZRegisters::Monitor1_aAngle]);
const int32_t drive1Revs = static_cast<int32_t>(map[TGZRegisters::Monitor1_aRevol]);

const float revolutions = drive1Angle / static_cast<float>(static_cast<uint32_t>(0xffffffff)) + static_cast<float>(drive1Revs);
```

The key here is to correctly use signed and unsigned integers, mainly for storing the `drive1Angle` and when casting the maximum value to float.
