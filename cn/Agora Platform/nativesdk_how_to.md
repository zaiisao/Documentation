
---
title: Native SDK 相关
description: 
platform: Native SDK 相关
updatedAt: Fri Nov 02 2018 04:06:57 GMT+0000 (UTC)
---
# Native SDK 相关
### setLogFilter最佳实践

该方法设置SDK输出日志的过滤器，不同过滤器可以使用不同的组合，日志级别顺序依次为 OFF、CRITICAL、ERROR、WARNING、INFO 和 DEBUG。

设置过滤器等级：

LOG_FILTER_OFF(0)：不输出任何日志
LOG_FILTER_DEBUG(0x80f)：输出所有的 API 日志
LOG_FILTER_INFO(0x0f)：输出 CRITICAL、ERROR、WARNING、INFO 级别的日志
LOG_FILTER_WARNING(0x0e)：仅输出 CRITICAL、ERROR、WARNING 级别的日志
LOG_FILTER_ERROR(0x0c)：仅输出 CRITICAL、ERROR 级别的日志
LOG_FILTER_CRITICAL(0x08)：仅输出 CRITICAL 级别的日志
最佳实践是默认配置，即只设置setLogfile：setInt（ “rtc.log_filter”, filter&LOG_FILTER_MASK ）只打印INFO+ERROR+WARNING日志，不输出DEBUG。

