# <center> PortSIP PBX Administration Guide </center>

Version: v16.0.0
Date: Sep 5, 2022

Copyright ©2022, PortSIP Solutions, Inc. All rights reserved. No part of this document may be reproduced, translated into another language or format, or transmitted in any form or by any means, electronic or mechanical, for any purpose, without the express written permission of PortSIP Solutions, Inc.

## Copyright Notice

Copyright© 2022 PortSIP Solutions, Inc.
All rights reserved.
Any technical documentation that is made available by PortSIP Solutions, Inc. is proprietary and
confidential and is considered the copyrighted work of PortSIP Solutions, Inc.
This publication is for distribution under PortSIP non-disclosure agreement only. No
part of this publication may be duplicated without the express written permission of
PortSIP Solutions, Inc.
PortSIP reserves the right to make changes without prior notice.

## Trademarks

 ![](..\images\25.png)

PortSIP®, the PortSIP logo and the names and marks associated with PortSIP products are trademarks and/or service marks of PortSIP Solutions, Inc. and are registered and/or common law marks in the United States and various other countries. All other trademarks are property of their respective owners. No portion hereof may be reproduced or transmitted in any form or by any means, for any purpose other than the recipient's personal use, without the express written permission of PortSIP.

## End User License Agreement

By installing, copying, or otherwise using this product, you acknowledge that you have read, understand and agree to be bound by the terms and conditions of the [PortSIP End User License Agreement](https://support.portsip.com/license-agreement/portsip-software-end-user-license-agreement) for this product.

## Open Source Software Used in this Product

This product may contain open source software. You may receive the open source software from PortSIP up to three(3) years after the distribution date of the applicable product or software at a charge not greater than the cost to PortSIP of shipping or distributing the software to you.

## Disclaimer

While PortSIP uses reasonable efforts to include accurate and up-to-date information in this document, PortSIP makes no warranties or representations as to its accuracy. PortSIP assumes no liability or responsibility for any typographical or other errors or omissions in the content of this document.

## Limitation of Liability

PortSIP and/or its respective suppliers make no representations about the suitability of the information contained in this document for any purpose. Information is provided “as is” without warranty of any kind and is subject to change without notice. The entire risk arising out of its use remains with the recipient. In no event shall PortSIP and/or its respective suppliers be liable for any direct, consequential, incidental, special, punitive or other damages whatsoever (including without limitation, damages for loss of business profits, business interruption, or loss of business information), even if PortSIP has been advised of the possibility of such damages.

## Summary of Changes

### Changes for Release v16.0.0

The following changes are included in this release:

- Rewrite all REST API

## About This Guide

This document provides guidelines to help facilitate the administration of the
PortSIP PBX Unified Communications solution. It includes important sections detailing installation, administration, and upgrade procedures in particular for the admin mode.
Where applicable, it lists and references other guides that contain detailed information on administrative procedures for BroadWorks servers or client applications.

## Overview

PortSIP PBX is a modern, complete Unified Communications solution, providing a
comprehensive suite of services addressing both business and consumer needs. The solution includes the following features:

- Multi Tenant
- Dealers Management
- Audio Calling and Video Calling
- Conferencing
- Instant Messaging and Presence (IM&P)
- Voice and Video Messages
- File and Picture Share
- Service Management (call settings)
- Audio and Video Call Recording
- Recordings Management
- Desktop Share
- Address Books/Contact Management
- Push Notifications
- Billing
- Virtual Receptionst
- Ring Group
- Contact Center
- Queue Callback
- Call Report
- Call Park
- Music On Hold
- Call Pickup Group
- Voicemail and Shared Voicemail
- Automatic Callback
- WebRTC
- Microsoft Teams Direct Routing
- Integrated SBC
- Full Opened REST API
- Zero Touch Provisioning
- Custom Template
- Role and Permissions
- Trunk Management
- Centralized Service Configuration (Call Forwarding, Do not Disturb, and so on).
- Troubleshooting
- Free Client VoIP SDK
- Free Client apps

## 1 Architecture

 ![](../images/pbx_diagram_v16.drawio.png)

 