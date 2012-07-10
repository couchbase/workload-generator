TMP_DIR = ./tmp
TMP_VER = $(TMP_DIR)/version_num.tmp

COUCHBASE_PYTHON_CLIENT = ../couchbase-python-client/couchbase \
                          ../couchbase-python-client/httplib2 \
                          ../couchbase-python-client/simplejson \
                          ../couchbase-python-client/uuid.py

default:

pythonlibdir=$(libdir)/python

pythonlib_SCRIPTS= cbworkloadgen

PYTHON_TOOLS= wrapper/cbworkloadgen

${PYTHON_TOOLS}: wrapper/wrapper
	cp $< $@

CLEANFILES = ${PYTHON_TOOLS}
bin_SCRIPTS = ${PYTHON_TOOLS}

EXTRA_DIST = $(pythonlib_SCRIPTS)

install-data-hook:
	cp -rf $(COUCHBASE_PYTHON_CLIENT) $(pythonlibdir)

clean-local:
	rm -f workload-generator*tar.gz
	rm -rf $(TMP_DIR)

bdist:
	test -d $(TMP_DIR) || mkdir $(TMP_DIR)
	git describe | sed s/-/_/g > $(TMP_VER)
	rm -f ./workload-generator_*.tar.gz
	rm -rf $(TMP_DIR)/cbworkloadgen
	mkdir -p $(TMP_DIR)/workload-generator
	cp -rf $(COUCHBASE_PYTHON_CLIENT) $(TMP_DIR)/workload-generator
	cp $(pythonlib_SCRIPTS) $(TMP_DIR)/workload-generator
	(cd $(TMP_DIR); tar cf - workload-generator) | gzip -9 > workload-generator_`cat $(TMP_VER)`-`uname -s`.`uname -m`.tar.gz
	echo created workload-generator_`cat $(TMP_VER)`-`uname -s`.`uname -m`.tar.gz
	rm -rf $(TMP_DIR)
