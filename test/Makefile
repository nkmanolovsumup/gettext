MODULES=gettext_demo

EBIN_FILES=$(MODULES:%=../ebin/%.beam)
ERLC_FLAGS+=+debug_info

tmp_name=tmp
dir=.
def_lang=en

all: pot test

po: $(dir)/lang/default/$(def_lang)/gettext.po

%.po: $(dir)/lang/default/$(def_lang)/gettext.pot
	msginit -i $< -o $@

pot: $(dir)/lang/default/$(def_lang)/gettext.pot

$(dir)/lang/default/$(def_lang)/gettext.pot: $(EBIN_FILES)
	erl -noshell -pa ../ebin -s gettext_compile epot2po
	install -D $(dir)/lang/$(tmp_name)/$(def_lang)/gettext.po \
	  $(dir)/lang/default/$(def_lang)/gettext.pot; \

clean:
	rm -rf lang/$(tmp_name)
	rm -f $(dir)/lang/default/$(def_lang)/gettext.pot
	rm -f gettext_server_db.dets
	rm -f $(EBIN_FILES)

../ebin/%.beam: %.erl $(INCLUDES) Makefile
	erlc -pa ../ebin -o ../ebin $(ERLC_FLAGS) +gettext $<

test: $(EBIN_FILES) pot
	@erl -pa ../ebin -noshell \
	  -eval 'gettext_demo:hello_world()' \
	  -s init stop
