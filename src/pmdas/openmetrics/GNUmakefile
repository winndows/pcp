#
# Copyright (c) 2017 Ronak Jain.
# Copyright (c) 2017,2019-2020 Red Hat.
#
# This program is free software; you can redistribute it and/or modify it
# under the terms of the GNU General Public License as published by the
# Free Software Foundation; either version 2 of the License, or (at your
# option) any later version.
#
# This program is distributed in the hope that it will be useful, but
# WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY
# or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License
# for more details.
#

TOPDIR = ../../..
include $(TOPDIR)/src/include/builddefs

IAM	= openmetrics
PYSCRIPT = pmda$(IAM).python
LDIRT	= domain.h root pmns $(IAM).log

MAN_SECTION = 1
MAN_PAGES = pmda$(IAM).$(MAN_SECTION)
MAN_DEST = $(PCP_MAN_DIR)/man$(MAN_SECTION)

DOMAIN	= $(IAM)
PMDADIR	= $(PCP_PMDAS_DIR)/$(IAM)

default_pcp default ::	build-me

include $(BUILDRULES)

ifeq "$(PMDA_OPENMETRICS)" "true"
build-me:	check_domain

install_pcp install :: default
	$(INSTALL) -m 755 -d $(PMDADIR)
	$(INSTALL) -m 755 -d $(PMDADIR)/config.d
	$(INSTALL) -m 644 grafana.url $(PMDADIR)/config.d/grafana.url
	$(INSTALL) -m 755 Install Remove Upgrade $(PYSCRIPT) $(PMDADIR)
	$(INSTALL) -m 755 -d $(PCP_VAR_DIR)/config/pmlogconf/$(IAM)
	$(INSTALL) -m 644 pmlogconf.summary $(PCP_VAR_DIR)/config/pmlogconf/$(IAM)/summary
	@$(INSTALL_MAN)

check:: $(PYSCRIPT)
	$(PYLINT) $^

else
build-me:
install_pcp install:
endif

check_domain:	../../pmns/stdpmid
	$(DOMAIN_PYTHONRULE)

check:: $(MAN_PAGES)
	$(MANLINT) $^
