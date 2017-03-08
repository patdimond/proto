# proto

[![Build Status](https://travis-ci.org/emicklei/proto.png)](https://travis-ci.org/emicklei/proto)
[![Go Report Card](https://goreportcard.com/badge/github.com/emicklei/proto)](https://goreportcard.com/report/github.com/emicklei/proto)
[![GoDoc](https://godoc.org/github.com/emicklei/proto?status.svg)](https://godoc.org/github.com/emicklei/proto)

Package in Go for parsing and formatting Google Protocol Buffers [.proto files version 2 + 3] (https://developers.google.com/protocol-buffers/docs/reference/proto3-spec)

### usage as package

    parser := proto.NewParser(reader)
	definition, err := parser.Parse()
	if err != nil {
		log.Fatalln("proto parsing failed", err)
	}

	formatter := proto.NewFormatter(writer," ")
	formatter.Format(definition)

### usage of protofmt command

	> protofmt -help
		Usage of protofmt [flags] [path ...]
  		-w	write result to (source) file instead of stdout

### format style

#### indentation
***protofmt*** uses 2 spaces for indentation.
The Formatter type can be initialized with a different indentSeparator string.

#### comments
Parsing and formatting support two styles of comments.
Inline comments are written with 2 leading forward slashed ( // ).
If an inline comment is present after a statement then it is written with a leading space.
Multi-line comments are written with a header ( /*[newline] ) and a footer ( [space]*/[newline] ) on separate lines.

	/*
	Multi line
	comment
	 */
	message X {

	}

#### empty line separators
Different structural top level definitions (message,service,enum) are separated with an empty line.

	message Y {}
	message Z {}

	enum E {}
Comments are preceeded with an empty line unless it is defined after a statement.

	// nice message to send
	message Greeting {

		// what is said
		string content = 1; // first
	}

#### structural elements in top level definitions
Fields in messages and enums, rpc-s in services are all formatted in a columnar style with aligments.

  repeated sfixed32 packed_sfixed32 =  98 [packed = true];
  repeated sfixed64 packed_sfixed64 =  99 [packed = true];
  repeated    float packed_float    = 100 [packed = true];
  repeated   double packed_double   = 101 [packed = true];

This example shows that field types are right aligned.
Field names are left aligned.
Field sequence numbers are right aligned.

#### options
Embedded options (specified for fields) have a compact format without special alignment.

	message Defaults {
	  optional   bool default_bool   = 1 [default = true   ];
	  optional string default_string = 2 [default = "hello"];
	}

Top level options have right aligned names and values.

	option         optimize_for =           SPEED;
	option java_outer_classname = "UnittestProto";


#### RPCs in services
Request and Response types of rpc elements are left aligned.
Closing brackets are right aligned.

	service SearchService {

  	  // comment
	  rpc Search (SearchRequest) returns (       SearchResponse); // Search
	  rpc Find   (Finder       ) returns (stream Result        ); // Find
	}

### install

    go get -u -v github.com/emicklei/proto

© 2017, [ernestmicklei.com](http://ernestmicklei.com).  MIT License. Contributions welcome.