---
title: "go-mp3"
---
# go-mp3

一个纯go语言编写的mp3解码器，它没有引用任何外部模块。没有使用 cgo 来调用c代码，理论上效率会很高，但只是单核的解码器。

## 作者

[Hajime Hoshi](https://github.com/hajimehoshi) 日本人，它的其它作品：

* [Ebitengine](https://github.com/hajimehoshi/ebiten) - A dead simple 2D game engine for Go
* [Oto](https://github.com/hajimehoshi/oto) - A low-level library to play sound on multiple platforms


go.mod文件

```go
module github.com/hajimehoshi/go-mp3

go 1.14

require github.com/hajimehoshi/oto/v2 v2.1.0
```

oto 是因为要做例子才引用的。

## Decoder 过程

简单的理解 mp3文件由 一堆tag 和一堆 frame 构成，frame 本身也是由自己的 header + data 构成。

读取文件跳过前面的mp3文件tags部分，按frame抽取，使用 huffman 还原数据。

创建Decoder的过程，它在创建的时候尝试读取了一个 frame 大概是用来验证是否是合法的 mp3 文件。

```go
// NewDecoder decodes the given io.Reader and returns a decoded stream.
//
// The stream is always formatted as 16bit (little endian) 2 channels
// even if the source is single channel MP3.
// Thus, a sample always consists of 4 bytes.
func NewDecoder(r io.Reader) (*Decoder, error) {
	s := &source{
		reader: r,
	}
	d := &Decoder{
		source: s,
		length: invalidLength,
	}

	if err := s.skipTags(); err != nil {
		return nil, err
	}
	// TODO: Is readFrame here really needed?
	if err := d.readFrame(); err != nil {
		return nil, err
	}
	freq, err := d.frame.SamplingFrequency()
	if err != nil {
		return nil, err
	}
	d.sampleRate = freq

	if err := d.ensureFrameStartsAndLength(); err != nil {
		return nil, err
	}

	return d, nil
}

```
## 解码 frame
解码 frame 的过程：

```go
func (d *Decoder) readFrame() error {
	var err error
	d.frame, _, err = frame.Read(d.source, d.source.pos, d.frame)
	if err != nil {
		if err == io.EOF {
			return io.EOF
		}
		if _, ok := err.(*consts.UnexpectedEOF); ok {
			// TODO: Log here?
			return io.EOF
		}
		return err
	}
	d.buf = append(d.buf, d.frame.Decode()...)
	return nil
}
```

frame.Read 函数内部调用了 huffman 还原数据。
