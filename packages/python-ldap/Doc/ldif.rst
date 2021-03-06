.. % $Id: ldif.rst,v 1.5 2010/02/05 13:12:06 stroeder Exp $

#####################################
:mod:`ldif` LDIF parser and generator
#####################################

.. module:: ldif
   :synopsis: Parses and generates LDIF files
.. moduleauthor:: python-ldap project (see http://www.python-ldap.org/)


This module parses and generates LDAP data in the format LDIF.    It is
implemented in pure Python and does not rely on any  non-standard modules.
Therefore it can be used stand-alone without  the rest of the python-ldap
package.

.. seealso::

   :rfc:`2849` - The LDAP Data Interchange Format (LDIF) - Technical Specification


.. _ldif-example:

Example
^^^^^^^^

The following example demonstrates how to write LDIF output
of an LDAP entry with :mod:`ldif` module.

>>> import sys,ldif
>>> entry={'objectClass':['top','person'],'cn':['Michael Stroeder'],'sn':['Stroeder']}
>>> dn='cn=Michael Stroeder,ou=Test'
>>> ldif_writer=ldif.LDIFWriter(sys.stdout)
>>> ldif_writer.unparse(dn,entry)
dn: cn=Michael Stroeder,ou=Test
cn: Michael Stroeder
objectClass: top
objectClass: person
sn: Stroeder


The following example demonstrates how to parse an LDIF file
with :mod:`ldif` module, skip some entries and write the result to stdout. ::

   import sys
   from ldif import LDIFParser, LDIFWriter

   skip_dn = ["uid=foo,ou=People,dc=example,dc=com", 
      "uid=bar,ou=People,dc=example,dc=com"]

   class MyLDIF(LDIFParser):
      def __init__(self, input, output):
         LDIFParser.__init__(self, input)
         self.writer = LDIFWriter(output)

      def handle(self, dn, entry):
         for i in skip_dn:
            if i == dn: return
         self.writer.unparse(dn, entry)

   parser = MyLDIF(open("input.ldif", 'rb'), sys.stdout)
   parser.parse()

