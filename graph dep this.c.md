[flowchart](https://mermaid-js.github.io/mermaid/#/flowchart)

```mermaid
graph LR
	this.h(this.h)
	this.c(this.c)
	this.o(this.o)
	this(this)

	cc.v[/cc --version/]

	this.o.do(this.o.do)
	this.o.dep[[this.o.dep]]

	this-o.do(this.do<br/>uses .o)
	this-o.dep[[this.dep<br/>sums .o]]

	this-c.do(this.do<br/>uses .c)
	this-c.dep[[this.dep<br/>sums .c]]


	this.o.do --> this.o.dep
	this-o.do --> this-o.dep
	this.h & this.c --> this.o.dep & this-c.dep
	this.o.dep ==> this.o --> this-o.dep ==> this
	this-c.dep ==> this
	cc.v --> this.o.dep & this-c.dep & this-o.dep
	this-c.do --> this-c.dep
```
