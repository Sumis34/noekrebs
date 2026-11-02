+++
date = '2026-11-02T20:24:34+02:00'
draft = true
title = 'Wake a Sony STR-DN840 from Standby with Wake-on-LAN'
+++

![Sony STR-DN840](images/61IuCxEdyNL.jpg)

While improving my home audio setup, I wanted to automate powering on my Sony receiver. Online research turned up plenty of suggestions, but none worked for me, or they only worked if the device was already switched on.

## Approaches that didn't work

One suggestion was the ScalarWebApi, which Sony devices serve and which can be used to control the device and its settings. A Python library for this interface, `python-songpal`, exists, but I had trouble getting it to work and eventually abandoned the approach.

## Finding out what the official app does

The Sony Music Center app can turn the receiver on remotely, so I figured that recording its network traffic might show me which requests it sends. I captured my phone's traffic with `PCAPdroid`, an app that records Android traffic to PCAP files without requiring a rooted phone.

## The solution

The capture showed that the app sends a Wake-on-LAN (WoL) magic packet to the receiver. This surprised me, because my earlier searches claimed that Sony's `Remote Start` feature doesn't use standard WoL packets. That turned out to be only half right: it does use a standard magic packet, but on a non-standard port. Instead of the usual port `9`, it uses `2304`.

With that discovery, a simple WoL command was enough to wake the receiver from standby. I then integrated it into Home Assistant using the WoL integration.
