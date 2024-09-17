---
title: "Gorm 数据库操作"
date: 2024-01-26 13:44:12 +08:00
draft: false
categories: [Golang]
tags: []
card: false
weight: 0
---

## RawMessage Scan&Value

`json.RawMessage` 以`[]byte`形式存储json数据，但在父结构体marshal和unmarshal时不会重复序列化，仅仅将数据复制到新json字符串中

通过继承Scanner和Valuer实现结构体变量写入和读出数据库

```go
type JSON json.RawMessage

func (j *JSON) Scan(value any) error {
	bytes, ok := value.([]byte)
	if !ok {
		return errors.New(fmt.Sprint("Failed to unmarshal JSONB value:", value))
	}
	
	result := json.RawMessage{}
	err := json.Unmarshal(bytes, &result)
	*j = JSON(result)
	return err
}

func (j JSON) Value() (driver.Value, error) {
	if len(j) == 0 {
		return nil, nil
	}
	return json.RawMessage(j).MarshalJSON()
}
```

## map Scan&Value

```go
type AllowAppsModel map[string]bool

func (a *AllowAppsModel) Scan(value any) error {
	bytes, ok := value.([]byte)
	if !ok {
		return errors.New(fmt.Sprint("Failed to unmarshal JSONB value:", value))
	}
	err := json.Unmarshal(bytes, a)
	return err
}

func (a AllowAppsModel) Value() (driver.Value, error) {
	return json.Marshal(a)
}
```
