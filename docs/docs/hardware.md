---
id: hardware
title: Recommended hardware
---

## Cameras

Cameras that output H.264 video and AAC audio will offer the most compatibility with all features of Frigate and Home Assistant. It is also helpful if your camera supports multiple substreams to allow different resolutions to be used for detection, streaming, and recordings without re-encoding.

I recommend Dahua, Hikvision, and Amcrest in that order. Dahua edges out Hikvision because they are easier to find and order, not because they are better cameras. I personally use Dahua cameras because they are easier to purchase directly. In my experience Dahua and Hikvision both have multiple streams with configurable resolutions and frame rates and rock solid streams. They also both have models with large sensors well known for excellent image quality at night. Not all the models are equal. Larger sensors are better than higher resolutions; especially at night. Amcrest is the fallback recommendation because they are rebranded Dahuas. They are rebranding the lower end models with smaller sensors or less configuration options.

Here are some of the camera's I recommend:

- [Loryta(Dahua) T5442TM-AS-LED](https://www.amazon.com/Loryta-IPC-T5442TM-AS-LED-Starlight-Eyeball-Network/dp/B07S5QZJDH/)
- [Loryta(Dahua) IPC-T5442TM-AS](https://www.amazon.com/Loryta-IPC-T5442TM-AS-Starlight-Eyeball-Network/dp/B07S21FVC7/)
- [Amcrest IP5M-T1179EW-28MM](https://www.amazon.com/Amcrest-5-Megapixel-NightVision-Weatherproof-IP5M-T1179EW-28MM/dp/B083G9KT4C/)

## Server

My current favorite is the Minisforum GK41 because the dual NICs allow you to setup a dedicated private network for your cameras where they can be blocked from accessing the internet.

| Name                    | Inference Speed | Notes                                                                                                                         |
| ----------------------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Minisforum GK41         | 9-10ms          | Great alternative to a NUC with dual Gigabit NICs. Easily handles several 1080p cameras.                                      |
| Intel NUC NUC7i3BNK     | 8-10ms          | Great performance. Can handle many cameras at 5fps depending on typical amounts of motion.                                    |
| BMAX B2 Plus            | 10-12ms         | Good balance of performance and cost. Also capable of running many other services at the same time as frigate.                |
| Atomic Pi               | 16ms            | Good option for a dedicated low power board with a small number of cameras. Can leverage Intel QuickSync for stream decoding. |
| Raspberry Pi 3B (32bit) | 60ms            | Can handle a small number of cameras, but the detection speeds are slow due to USB 2.0.                                       |
| Raspberry Pi 4 (32bit)  | 15-20ms         | Can handle a small number of cameras. The 2GB version runs fine.                                                              |
| Raspberry Pi 4 (64bit)  | 10-15ms         | Can handle a small number of cameras. The 2GB version runs fine.                                                              |

## Google Coral TPU

It is strongly recommended to use a Google Coral. Frigate is designed around the expectation that a Coral is used to achieve very low inference speeds. Offloading TensorFlow to the Google Coral is an order of magnitude faster and will reduce your CPU load dramatically. A $60 device will outperform $2000 CPU. Frigate should work with any supported Coral device from https://coral.ai

The USB version is compatible with the widest variety of hardware and does not require a driver on the host machine. However, it does lack the automatic throttling features of the other versions.

The PCIe and M.2 versions require installation of a driver on the host. Follow the instructions for your version from https://coral.ai

A single Coral can handle many cameras and will be sufficient for the majority of users. You can calculate the maximum performance of your Coral based on the inference speed reported by Frigate. With an inference speed of 10, your Coral will top out at `1000/10=100`, or 100 frames per second. If your detection fps is regularly getting close to that, you should first consider tuning motion masks. If those are already properly configured, a second Coral may be needed.