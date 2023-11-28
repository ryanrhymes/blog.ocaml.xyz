.PHONY: clean
clean:
	rm -rf _site .jekyll-cache

.PHONY: push
push:
	git commit -am "coding ..." && \
	git push origin `git branch | grep \* | cut -d ' ' -f2`

.PHONY: build
build:
	JEKYLL_ENV=production jekyll build

.PHONY: publish
publish: build
	cp -r _site/* ../docs/
	rm ../docs/Makefile
	git add ../docs


### databricks fs cp -r dbfs:/user/liang.wang/output/ocaml.xyz/ _posts/
### databricks fs rm -r dbfs:/user/liang.wang/output/ocaml.xyz/
### databricks fs ls dbfs:/user/liang.wang/output/ocaml.xyz