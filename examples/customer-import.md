# Example: Customer Import

## Original Request

```md
帮我做一个客户导入功能。
```

## ReqCheck Questions

```md
ReqCheck 会先帮你想清楚：

1. 用户上传后，是直接加入客户列表，还是先看到一页确认名单？
2. 文件里有重复客户时，是自动跳过，还是提醒用户决定？
3. 只导入成功一部分时，用户要不要看到成功和失败的明细？
```

## Brief Outline

```md
# Development Brief

## Goal
Let users import customers from a file without accidentally creating wrong or duplicate customer records.

## Not Doing
No complex field-mapping screen in version one.

Risk if skipped:
Some files may have different column names and fail import.

Guardrail:
Provide a simple file template and clear error messages.

## User Path
Upload file -> preview detected customers -> confirm import -> see success and failure details.

## Data and Rules
Existing customers should not be silently overwritten.

## Edge Cases
Duplicate customer, invalid row, partial import failure, empty file, wrong file type.
```
