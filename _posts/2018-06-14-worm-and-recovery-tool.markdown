---
layout: post
title:  "WORM and the WORM Recovery Tool"
date:   2018-06-14 15:05:00 -0800
categories: software front
featured-image: worm-stornext.jpg
hide-featured-image: true
---
These projects enable the storage of thousands of terabytes of sensitive, encrypted archival data in a way that ensures it can be recovered by customers, even into the far future or after the data's custodian goes out of business.<!--more-->

## Summary

The WORM ("Write Once, Read Only") project was started at Global Relay in response to a customer desire to have a final backup copy of all their raw data stored in an encrypted form that could be recovered in the case of a disaster such as Global Relay going out of business. WORM refers to the project that takes archived and real-time data at a rate of hundreds of gigabytes per day and writes it to a LTO tape archive, with the encryption keys written to separate tapes that are periodically sent to the customer for them to keep safe. The WORM Recovery Tool is a utility for combining the keys with the tapes and decrypting the raw data at a rate of around a terabye per day.

WORM is written in Java, deployed using Ansible, tested using TestNG, and operates on a network of dedicated CentOS systems. The WORM Recovery Tool is written in C# and tested using Python. It is used in production today to write several gigabytes of data to tape, with the capacity to add many more similarly-sized customers.

## My Contribution

I was involved in the earliest development stages of both projects, contributing to the first release of the Recovery Tool and the first two releases of WORM. On WORM, I wrote several features, many integration and end-to-end tests, and much of the Ansible deployment system. On the Recovery Tool, I wrote many of the features and tests.

## Further Details

Global Relay treats the WORM projects as just one part of the wider suite of Archive offerings. There is no specific public literature about it but information about GR's Archive is available [here][gr-archive].

[gr-archive]: https://www.globalrelay.com/gr-services/archive
