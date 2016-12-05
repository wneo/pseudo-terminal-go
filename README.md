# pseudo-terminal-go

## Install
```
go get github.com/acewu/pseudo-terminal-go/terminal
```

## Run a test
```
go build test.go
./test
```


		term, err := terminal.NewWithStdInOut()
		if err != nil {
			panic(err)
		}
		defer term.ReleaseFromStdInOut() // defer this
		term.SetPrompt("> ")

		var line string
		for {
			if err == io.EOF {
				return
			} else if line == "^C" {
				return
			}
			if (err != nil && strings.Contains(err.Error(), "control-c break")) || len(line) == 0 {
				line, err = term.ReadLine()
			} else {
				line, err = term.ReadLine()
			}
			line = strings.TrimSpace(line)
			if len(line) > 0 {
				tokens := strings.Split(line, " ")
				if handler, ok := c.handlers[tokens[0]]; ok {
					code, msg := handler(tokens)
					term.Write([]byte(msg))
					if code != 0 {
						break
					}
				} else {
					if len(tokens[0]) > 0 {
						s := fmt.Sprintf("command not found: %s\n", tokens[0])
						term.Write([]byte(s))
					}
				}
			}
		}