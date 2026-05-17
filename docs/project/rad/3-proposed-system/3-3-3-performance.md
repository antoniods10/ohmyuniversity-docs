---
title: RAD - 3.3.3 Performance | OhMyUniversity!
description: Performance requirements of OhMyUniversity - defining response time thresholds and resource efficiency targets for optimized mobile and web usage.
head:
  - - meta
    - property: og:title
      content: RAD - 3.3.3 Performance | OhMyUniversity!
  - - meta
    - property: og:description
      content: Performance requirements of OhMyUniversity - defining response time thresholds and resource efficiency targets for optimized mobile and web usage.
  - - meta
    - property: og:url
      content: https://docs.university.ohmyopensource.org/project/rad/3-proposed-system/3-3-3-performance
  - - meta
    - name: keywords
      content: ohmyuniversity, rad, performance, nonfunctional requirements, response time, latency, resource efficiency, battery consumption, cineca, mobile app, university app, middleware
  - - meta
    - name: twitter:title
      content: RAD - 3.3.3 Performance | OhMyUniversity!
  - - meta
    - name: twitter:description
      content: Performance requirements of OhMyUniversity - defining response time thresholds and resource efficiency targets for optimized mobile and web usage.
---

# OhMyUniversity! - RAD: 3.3.3 Performance

The performance requirements define the expected speed and resource efficiency of **OhMyUniversity!** during normal usage. Since the system is designed for frequent access from mobile and web devices, it must provide fast response times while limiting the consumption of device resources.

- **Response Time (Latency):** The loading of the university student record via the Cineca API must be completed within a maximum time of 2.0 seconds in 95% of requests made under 4G/5G connectivity.

- **Resource Efficiency:** The application should be optimized to limit unnecessary consumption of device resources, including battery, memory, and network usage, especially during prolonged use on mobile devices. 
