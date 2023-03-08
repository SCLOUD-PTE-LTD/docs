---
layout: default
title: ulive
parent: Media Services
grand_parent: Public Cloud
permalink: /public-cloud/media-services/ulive/
nav_order: 1
---
# ULive
## Product Introduction
ULive provides you with high-definition smooth, low-latency, high-concurrency audio and video cloud processing services. It includes core functions such as real-time transcoding, slice storage, and distribution acceleration. It provides a smooth access experience for end users and helps live video services go online quickly.
## Key concepts
### Push/pull flow:
- Streaming is the process of pushing live content to a server;
- Pulling is the process of pulling live content from the server and using a specified address.
### RTMP
RTMP is an acronym for Real Time Messaging Protocol. The protocol is based on TCP and is a protocol family, including the basic protocol of RTMP and various variants such as RTMPPT/RTMPS/RTMPE. RTMP is a network protocol designed for real-time data communication, mainly used for audio, video and data communication between Flash/AIR platforms and streaming media/interactive servers that support RTMP protocol.
### HLS
HLS (HTTP Live Streaming), Apple's dynamic bitrate adaptive technology. It is mainly used for audio and video services of PC and Apple terminals. Includes an m3u(8) index file, TS media fragment file, and key encrypted string file.
## Core functionality
### Multiple transport protocols
Support RTMP and HLS protocols: RTMP is suitable for low-latency and highly interactive scenarios, and HLS is suitable for high-concurrency and low-interaction scenarios.
### Real-time transcoding of live streams
Provides video and audio transcoding functions to meet the various needs of different customers for transcoding scenarios.
### Record to on-demand
Supports converting live recordings to mp4 and HLS on-demand files, automatically repairs interrupted video content, and provides flexible file saving policies.
### Access authentication
Supports the authentication function of the playback URL encryption method to verify user access rights and reduce the risk of content theft and cost loss.
### Push authentication
Provides an authentication mechanism for pushing source content to avoid unauthorized content push and playback and cost losses caused by resource theft.

## Live streaming process
Live streaming flowchart:

Typical live streaming process: 

- Live content collection, support for streaming on any RTMP device, including the use of user-developed push devices, and the pushed video stream is accelerated through CDN edge nodes to ensure the stability of uplink transmission.
- Live broadcast system processing, live content pushed to SCloud live broadcast system, video transcoding, screenshots, recording and other processing can be performed on demand, and the distributed storage cluster can process massive data to ensure security and stability.
- Live content playback, processed video content can be distributed to the audience's devices through the CDN distribution system all over the country. The playback device can use the user-developed playback device.

## Features and benefits
Push authentication Access authentication Protect content property rights
Provides push authentication policies to prevent illegal push streams. Supports access authentication policies, verifies user access rights, and reduces the risk of content theft and cost loss.

High concurrency and large traffic support millions of users to watch
The CDN intelligent scheduling mechanism ensures that compressed video sources are quickly distributed to edge nodes, reducing the pressure on origin servers. 200+ acceleration nodes and high-quality bandwidth carry millions of users to access and watch at the same time.

## Application scenarios
### Social streaming
More and more streamer users are sharing and interacting with other connected users through live video streaming software. However, due to the wide distribution of users and the great differences in network conditions in different places, many users have lag and delay when using social live broadcasting, which greatly affects the quality of social networking. SCloud Live Cloud (ULive) adopts distributed push nodes and download nodes to better support individual users to upload and watch live broadcasts. At the same time, it supports HDL protocol playback, which shortens the load time of live broadcast to the greatest extent possible. From the network layer to the application layer, the live broadcast quality is optimized in an all-round way to greatly improve user experience satisfaction.
### Live media broadcasting
Live broadcasts of sports events, major events, entertainment and other events have gradually moved closer to the Internet. Viewers can watch the event in real time through the live streaming software, and even participate in the interaction of the event. However, because live media often has explosive growth characteristics, this has extremely high requirements for the scalability and flexibility of live broadcasting. SCloud Live Cloud (ULive) has more than 200 CDN acceleration nodes across the country, which can support millions of people to watch online at the same time and expand the capacity at any time. At the same time, live recording is supported, and the content after the event live recording can be converted to on-demand for playback. The CDN acceleration network and SCloud's highly reliable cloud environment throughout the country provide double protection for every live event, so that every viewer does not miss a single detail.
### Live show
Anchor users can show singing, dance and other performance forms to all audience users through live broadcast software, and interact with the audience. If there is a lag phenomenon during the live broadcast, it will greatly affect the audience's experience. At the same time, the monitoring and management of show content has become increasingly prominent. SCloud Live Cloud (ULive) adopts multi-bitrate output technology, so that viewers in areas with poor network quality can enjoy a smooth live broadcast playback experience. At the same time, ULive's real-time live screenshot function can more conveniently monitor live content in real time, and realize second-level banning, real-time interruption and banning of unhealthy content.
### Game streaming
Game live streaming is a real-time interactive entertainment for video games, which requires high viewing instructions and details of game operation instructions. The live broadcast process needs to be clear and smooth, with good continuity and clarity. SCloud Live Cloud (ULive) supports multiple transmission protocols, even if audience users are distributed across the country, they can enjoy a smooth and clear game live broadcast experience through multiple forms of terminals.