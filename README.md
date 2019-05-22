# `graphite_encoderl`
[Graphite line](https://graphite.readthedocs.io/en/latest/feeding-carbon.html#the-plaintext-protocol) encoder in Erlang.  


## Build
```sh
~ $ git clone --depth 1 git/address/of/graphite_encoderl && cd graphite_encoderl
...
~/graphite_encoderl $ make
```

## Usage
```sh
~/graphite_encoderl $ make shell
Erlang/OTP 21 [erts-10.1] [source] [64-bit] [smp:4:4] [ds:4:4:10] [async-threads:256] [hipe]

Eshell V10.1  (abort with ^G)
```
```erlang
%  Yields iolist and appends timestamp:
1> graphite_encoderl:encode({key, 10}).
["key"," ","10"," ","1558553669","\n"]

%  Yields string and appends timestamp:
2> graphite_encoderl:encode({key, 3.140000000}, #{return_type => string}).
"key 3.14 1558553699\n"

%  Yields binary and places timestamp:
3> graphite_encoderl:encode({key, 3.140000000, 1558553694}, #{return_type => binary}).
<<"key 3.14 1558553694\n">>

%  Yields string and encodes keys and appends timestamp:
4> graphite_encoderl:encode({[key1, "key2", <<"key3">>], 100}, #{return_type => string}).                    
"key1.key2.key3 100 1558553784\n"

%            Yields encoded lines:
5> io:format(graphite_encoderl:encode([{"key", X} || X <- lists:seq(1, 100, 10)], #{return_type => string})).
key 1 1558553836
key 11 1558553836
key 21 1558553836
key 31 1558553836
key 41 1558553836
key 51 1558553836
key 61 1558553836
key 71 1558553836
key 81 1558553836
key 91 1558553836
ok

%  Also you can use maps instead of lists (bu timestamp is not allowed):
6> graphite_encoderl:encode(#{"key1" => 1, <<"key2">> => 2}, #{return_type => binary}).                      
<<"key1 1 1558554061\nkey2 2 1558554061\n">>
```

#### Author
`pouriya.jahanbakhsh@gmail.com`
