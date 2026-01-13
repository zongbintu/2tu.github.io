title: manjaro安装lxml报错解决
tags: [lxml]
date: 2026-01-13 16:27:27
categories: Python
---
OS: Manjaro xfc
Python: 3.13.11
lxml: 4.9.7
通过requirements.txt文件安装报错
```shell
[output] Collecting lxml==4.9.4
[output]   Using cached lxml-4.9.4.tar.gz (3.6 MB)
[output]   Installing build dependencies: started
[output]   Installing build dependencies: finished with status 'done'
[output]   Getting requirements to build wheel: started
[output]   Getting requirements to build wheel: finished with status 'done'
[output]   Preparing metadata (pyproject.toml): started
[output]   Preparing metadata (pyproject.toml): finished with status 'done'
[output] Collecting openpyxl==2.5.12
[output]   Using cached openpyxl-2.5.12-py2.py3-none-any.whl
[output] Collecting requests==2.31.0
[output]   Using cached requests-2.31.0-py3-none-any.whl.metadata (4.6 kB)
[output] Collecting jdcal (from openpyxl==2.5.12)
[output]   Using cached jdcal-1.4.1-py2.py3-none-any.whl.metadata (5.0 kB)
[output] Collecting et_xmlfile (from openpyxl==2.5.12)
[output]   Using cached et_xmlfile-2.0.0-py3-none-any.whl.metadata (2.7 kB)
[output] Collecting charset-normalizer<4,>=2 (from requests==2.31.0)
[output]   Using cached charset_normalizer-3.4.4-cp313-cp313-manylinux2014_x86_64.manylinux_2_17_x86_64.manylinux_2_28_x86_64.whl.metadata (37 kB)
[output] Collecting idna<4,>=2.5 (from requests==2.31.0)
[output]   Using cached idna-3.11-py3-none-any.whl.metadata (8.4 kB)
[output] Collecting urllib3<3,>=1.21.1 (from requests==2.31.0)
[output]   Using cached urllib3-2.6.3-py3-none-any.whl.metadata (6.9 kB)
[output] Collecting certifi>=2017.4.17 (from requests==2.31.0)
[output]   Using cached certifi-2026.1.4-py3-none-any.whl.metadata (2.5 kB)
[output] Using cached requests-2.31.0-py3-none-any.whl (62 kB)
[output] Using cached certifi-2026.1.4-py3-none-any.whl (152 kB)
[output] Using cached charset_normalizer-3.4.4-cp313-cp313-manylinux2014_x86_64.manylinux_2_17_x86_64.manylinux_2_28_x86_64.whl (153 kB)
[output] Using cached idna-3.11-py3-none-any.whl (71 kB)
[output] Using cached urllib3-2.6.3-py3-none-any.whl (131 kB)
[output] Using cached et_xmlfile-2.0.0-py3-none-any.whl (18 kB)
[output] Using cached jdcal-1.4.1-py2.py3-none-any.whl (9.5 kB)
[output] Building wheels for collected packages: lxml
[output]   Building wheel for lxml (pyproject.toml): started
[output]   Building wheel for lxml (pyproject.toml): finished with status 'error'
[output] Failed to build lxml
 [error]   error: subprocess-exited-with-error
 [error]   
 [error]   × Building wheel for lxml (pyproject.toml) did not run successfully.
 [error]   │ exit code: 1
 [error]   ╰─> [425 lines of output]
 [error]       <string>:67: UserWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html. The pkg_resources package is slated for removal as early as 2025-11-30. Refrain from using this package or pin to Setuptools<81.
 [error]       /tmp/pip-build-env-2w5tteta/overlay/lib/python3.13/site-packages/setuptools/dist.py:759: SetuptoolsDeprecationWarning: License classifiers are deprecated.
 [error]       !!
 [error]       
 [error]               ********************************************************************************
 [error]               Please consider removing the following classifiers in favor of a SPDX license expression:
 [error]       
 [error]               License :: OSI Approved :: BSD License
 [error]       
 [error]               See https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license for details.
 [error]               ********************************************************************************
 [error]       
 [error]       !!
 [error]         self._finalize_license_expression()
 [error]       Building lxml version 4.9.4.
 [error]       Building without Cython.
 [error]       Building against libxml2 2.15.1 and libxslt 1.1.45
 [error]       running bdist_wheel
 [error]       running build
 [error]       running build_py
 [error]       creating build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/usedoctest.py -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/sax.py -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/pyclasslookup.py -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/doctestcompare.py -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/cssselect.py -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/builder.py -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/_elementpath.py -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/__init__.py -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/ElementInclude.py -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       creating build/lib.linux-x86_64-cpython-313/lxml/includes
 [error]       copying src/lxml/includes/__init__.py -> build/lib.linux-x86_64-cpython-313/lxml/includes
 [error]       creating build/lib.linux-x86_64-cpython-313/lxml/html
 [error]       copying src/lxml/html/usedoctest.py -> build/lib.linux-x86_64-cpython-313/lxml/html
 [error]       copying src/lxml/html/soupparser.py -> build/lib.linux-x86_64-cpython-313/lxml/html
 [error]       copying src/lxml/html/html5parser.py -> build/lib.linux-x86_64-cpython-313/lxml/html
 [error]       copying src/lxml/html/formfill.py -> build/lib.linux-x86_64-cpython-313/lxml/html
 [error]       copying src/lxml/html/diff.py -> build/lib.linux-x86_64-cpython-313/lxml/html
 [error]       copying src/lxml/html/defs.py -> build/lib.linux-x86_64-cpython-313/lxml/html
 [error]       copying src/lxml/html/clean.py -> build/lib.linux-x86_64-cpython-313/lxml/html
 [error]       copying src/lxml/html/builder.py -> build/lib.linux-x86_64-cpython-313/lxml/html
 [error]       copying src/lxml/html/_setmixin.py -> build/lib.linux-x86_64-cpython-313/lxml/html
 [error]       copying src/lxml/html/_html5builder.py -> build/lib.linux-x86_64-cpython-313/lxml/html
 [error]       copying src/lxml/html/_diffcommand.py -> build/lib.linux-x86_64-cpython-313/lxml/html
 [error]       copying src/lxml/html/__init__.py -> build/lib.linux-x86_64-cpython-313/lxml/html
 [error]       copying src/lxml/html/ElementSoup.py -> build/lib.linux-x86_64-cpython-313/lxml/html
 [error]       creating build/lib.linux-x86_64-cpython-313/lxml/isoschematron
 [error]       copying src/lxml/isoschematron/__init__.py -> build/lib.linux-x86_64-cpython-313/lxml/isoschematron
 [error]       copying src/lxml/etree.h -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/etree_api.h -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/lxml.etree.h -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/lxml.etree_api.h -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/objectify.pyx -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/etree.pyx -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/xsltext.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/xslt.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/xpath.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/xmlschema.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/xmlid.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/xmlerror.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/xinclude.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/serializer.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/schematron.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/saxparser.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/relaxng.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/readonlytree.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/public-api.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/proxy.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/parsertarget.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/parser.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/objectpath.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/nsclasses.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/iterparse.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/extensions.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/dtd.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/docloader.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/debug.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/cleanup.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/classlookup.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/apihelpers.pxi -> build/lib.linux-x86_64-cpython-313/lxml
 [error]       copying src/lxml/includes/xslt.pxd -> build/lib.linux-x86_64-cpython-313/lxml/includes
 [error]       copying src/lxml/includes/xpath.pxd -> build/lib.linux-x86_64-cpython-313/lxml/includes
 [error]       copying src/lxml/includes/xmlschema.pxd -> build/lib.linux-x86_64-cpython-313/lxml/includes
 [error]       copying src/lxml/includes/xmlparser.pxd -> build/lib.linux-x86_64-cpython-313/lxml/includes
 [error]       copying src/lxml/includes/xmlerror.pxd -> build/lib.linux-x86_64-cpython-313/lxml/includes
 [error]       copying src/lxml/includes/xinclude.pxd -> build/lib.linux-x86_64-cpython-313/lxml/includes
 [error]       copying src/lxml/includes/uri.pxd -> build/lib.linux-x86_64-cpython-313/lxml/includes
 [error]       copying src/lxml/includes/tree.pxd -> build/lib.linux-x86_64-cpython-313/lxml/includes
 [error]       copying src/lxml/includes/schematron.pxd -> build/lib.linux-x86_64-cpython-313/lxml/includes
 [error]       copying src/lxml/includes/relaxng.pxd -> build/lib.linux-x86_64-cpython-313/lxml/includes
 [error]       copying src/lxml/includes/htmlparser.pxd -> build/lib.linux-x86_64-cpython-313/lxml/includes
 [error]       copying src/lxml/includes/etreepublic.pxd -> build/lib.linux-x86_64-cpython-313/lxml/includes
 [error]       copying src/lxml/includes/dtdvalid.pxd -> build/lib.linux-x86_64-cpython-313/lxml/includes
 [error]       copying src/lxml/includes/config.pxd -> build/lib.linux-x86_64-cpython-313/lxml/includes
 [error]       copying src/lxml/includes/c14n.pxd -> build/lib.linux-x86_64-cpython-313/lxml/includes
 [error]       copying src/lxml/includes/__init__.pxd -> build/lib.linux-x86_64-cpython-313/lxml/includes
 [error]       copying src/lxml/includes/lxml-version.h -> build/lib.linux-x86_64-cpython-313/lxml/includes
 [error]       copying src/lxml/includes/etree_defs.h -> build/lib.linux-x86_64-cpython-313/lxml/includes
 [error]       creating build/lib.linux-x86_64-cpython-313/lxml/isoschematron/resources/rng
 [error]       copying src/lxml/isoschematron/resources/rng/iso-schematron.rng -> build/lib.linux-x86_64-cpython-313/lxml/isoschematron/resources/rng
 [error]       creating build/lib.linux-x86_64-cpython-313/lxml/isoschematron/resources/xsl
 [error]       copying src/lxml/isoschematron/resources/xsl/XSD2Schtrn.xsl -> build/lib.linux-x86_64-cpython-313/lxml/isoschematron/resources/xsl
 [error]       copying src/lxml/isoschematron/resources/xsl/RNG2Schtrn.xsl -> build/lib.linux-x86_64-cpython-313/lxml/isoschematron/resources/xsl
 [error]       creating build/lib.linux-x86_64-cpython-313/lxml/isoschematron/resources/xsl/iso-schematron-xslt1
 [error]       copying src/lxml/isoschematron/resources/xsl/iso-schematron-xslt1/iso_svrl_for_xslt1.xsl -> build/lib.linux-x86_64-cpython-313/lxml/isoschematron/resources/xsl/iso-schematron-xslt1
 [error]       copying src/lxml/isoschematron/resources/xsl/iso-schematron-xslt1/iso_schematron_skeleton_for_xslt1.xsl -> build/lib.linux-x86_64-cpython-313/lxml/isoschematron/resources/xsl/iso-schematron-xslt1
 [error]       copying src/lxml/isoschematron/resources/xsl/iso-schematron-xslt1/iso_schematron_message.xsl -> build/lib.linux-x86_64-cpython-313/lxml/isoschematron/resources/xsl/iso-schematron-xslt1
 [error]       copying src/lxml/isoschematron/resources/xsl/iso-schematron-xslt1/iso_dsdl_include.xsl -> build/lib.linux-x86_64-cpython-313/lxml/isoschematron/resources/xsl/iso-schematron-xslt1
 [error]       copying src/lxml/isoschematron/resources/xsl/iso-schematron-xslt1/iso_abstract_expand.xsl -> build/lib.linux-x86_64-cpython-313/lxml/isoschematron/resources/xsl/iso-schematron-xslt1
 [error]       copying src/lxml/isoschematron/resources/xsl/iso-schematron-xslt1/readme.txt -> build/lib.linux-x86_64-cpython-313/lxml/isoschematron/resources/xsl/iso-schematron-xslt1
 [error]       running build_ext
 [error]       building 'lxml.etree' extension
 [error]       creating build/temp.linux-x86_64-cpython-313/src/lxml
 [error]       gcc -fno-strict-overflow -Wsign-compare -DNDEBUG -g -O3 -Wall -march=x86-64 -mtune=generic -O3 -pipe -fno-plt -fexceptions -Wp,-D_FORTIFY_SOURCE=3 -Wformat -Werror=format-security -fstack-clash-protection -fcf-protection -fno-omit-frame-pointer -mno-omit-leaf-frame-pointer -g -ffile-prefix-map=/build/python/src=/usr/src/debug/python -flto=auto -ffat-lto-objects -march=x86-64 -mtune=generic -O3 -pipe -fno-plt -fexceptions -Wp,-D_FORTIFY_SOURCE=3 -Wformat -Werror=format-security -fstack-clash-protection -fcf-protection -fno-omit-frame-pointer -mno-omit-leaf-frame-pointer -g -ffile-prefix-map=/build/python/src=/usr/src/debug/python -flto=auto -march=x86-64 -mtune=generic -O3 -pipe -fno-plt -fexceptions -Wp,-D_FORTIFY_SOURCE=3 -Wformat -Werror=format-security -fstack-clash-protection -fcf-protection -fno-omit-frame-pointer -mno-omit-leaf-frame-pointer -g -ffile-prefix-map=/build/python/src=/usr/src/debug/python -flto=auto -fPIC -DCYTHON_CLINE_IN_TRACEBACK=0 -I/usr/include/libxml2 -Isrc -Isrc/lxml/includes -I/home/project/test/.venv/include -I/usr/include/python3.13 -c src/lxml/etree.c -o build/temp.linux-x86_64-cpython-313/src/lxml/etree.o -w
 [error]       src/lxml/etree.c: In function ‘__Pyx_init_assertions_enabled’:
 [error]       src/lxml/etree.c:5988:39: error: implicit declaration of function ‘_PyInterpreterState_GetConfig’; did you mean ‘PyInterpreterState_GetID’? [-Wimplicit-function-declaration]
 [error]        5988 |     __pyx_assertions_enabled_flag = ! _PyInterpreterState_GetConfig(__Pyx_PyThreadState_Current->interp)->optimization_level;
 [error]             |                                       ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]             |                                       PyInterpreterState_GetID
 [error]       src/lxml/etree.c:5988:105: error: invalid type argument of ‘->’ (have ‘int’)
 [error]        5988 |     __pyx_assertions_enabled_flag = ! _PyInterpreterState_GetConfig(__Pyx_PyThreadState_Current->interp)->optimization_level;
 [error]             |                                                                                                         ^~
 [error]       src/lxml/etree.c: In function ‘__pyx_f_4lxml_5etree_fixThreadDictNamesForDtd’:
 [error]       src/lxml/etree.c:19193:49: error: passing argument 1 of ‘__pyx_f_4lxml_5etree__fixThreadDictPtr’ from incompatible pointer type [-Wincompatible-pointer-types]
 [error]       19193 |         __pyx_f_4lxml_5etree__fixThreadDictPtr((&__pyx_v_c_attribute->defaultValue), __pyx_v_c_src_dict, __pyx_v_c_dict);
 [error]             |                                                ~^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]             |                                                 |
 [error]             |                                                 xmlChar ** {aka unsigned char **}
 [error]       src/lxml/etree.c:18536:82: note: expected ‘const xmlChar **’ {aka ‘const unsigned char **’} but argument is of type ‘xmlChar **’ {aka ‘unsigned char **’}
 [error]       18536 | static CYTHON_INLINE void __pyx_f_4lxml_5etree__fixThreadDictPtr(const xmlChar **__pyx_v_c_ptr, xmlDict *__pyx_v_c_src_dict, xmlDict *__pyx_v_c_dict) {
 [error]             |                                                                  ~~~~~~~~~~~~~~~~^~~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__pyx_f_4lxml_5etree_14_ParserContext_prepare’:
 [error]       src/lxml/etree.c:113221:38: error: assignment to ‘xmlStructuredErrorFunc’ {aka ‘void (*)(void *, const struct _xmlError *)’} from incompatible pointer type ‘void (*)(void *, xmlError *)’ {aka ‘void (*)(void *, struct _xmlError *)’} [-Wincompatible-pointer-types]
 [error]       113221 |   __pyx_v_self->_c_ctxt->sax->serror = __pyx_f_4lxml_5etree__receiveParserError;
 [error]              |                                      ^
 [error]       src/lxml/etree.c:7472:13: note: ‘__pyx_f_4lxml_5etree__receiveParserError’ declared here
 [error]        7472 | static void __pyx_f_4lxml_5etree__receiveParserError(void *, xmlError *); /*proto*/
 [error]             |             ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]       In file included from /usr/include/libxml2/libxml/valid.h:19,
 [error]                        from /usr/include/libxml2/libxml/parser.h:24,
 [error]                        from /usr/include/libxml2/libxml/tree.h:20,
 [error]                        from src/lxml/includes/etree_defs.h:189,
 [error]                        from src/lxml/etree.c:864:
 [error]       /usr/include/libxml2/libxml/xmlerror.h:953:16: note: ‘xmlStructuredErrorFunc’ declared here
 [error]         953 | typedef void (*xmlStructuredErrorFunc) (void *userData, const xmlError *error);
 [error]             |                ^~~~~~~~~~~~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__pyx_f_4lxml_5etree_11_BaseParser__registerHtmlErrorHandler’:
 [error]       src/lxml/etree.c:117709:25: error: assignment to ‘xmlStructuredErrorFunc’ {aka ‘void (*)(void *, const struct _xmlError *)’} from incompatible pointer type ‘void (*)(void *, xmlError *)’ {aka ‘void (*)(void *, struct _xmlError *)’} [-Wincompatible-pointer-types]
 [error]       117709 |     __pyx_v_sax->serror = __pyx_f_4lxml_5etree__receiveParserError;
 [error]              |                         ^
 [error]       src/lxml/etree.c:113855:13: note: ‘__pyx_f_4lxml_5etree__receiveParserError’ declared here
 [error]       113855 | static void __pyx_f_4lxml_5etree__receiveParserError(void *__pyx_v_c_context, xmlError *__pyx_v_error) {
 [error]              |             ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]       /usr/include/libxml2/libxml/xmlerror.h:953:16: note: ‘xmlStructuredErrorFunc’ declared here
 [error]         953 | typedef void (*xmlStructuredErrorFunc) (void *userData, const xmlError *error);
 [error]             |                ^~~~~~~~~~~~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__pyx_pf_4lxml_5etree_11TreeBuilder_4data’:
 [error]       src/lxml/etree.c:137976:66: error: passing argument 1 of ‘__pyx_f_4lxml_5etree_11TreeBuilder__handleSaxData’ from incompatible pointer type [-Wincompatible-pointer-types]
 [error]       137976 |   __pyx_t_1 = __pyx_f_4lxml_5etree_11TreeBuilder__handleSaxData(((struct __pyx_obj_4lxml_5etree__SaxParserTarget *)__pyx_v_self), __pyx_v_data); if (unlikely(__pyx_t_1 == ((int)-1))) __PYX_ERR(3, 834, __pyx_L1_error)
 [error]              |                                                                 ~^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]              |                                                                  |
 [error]              |                                                                  struct __pyx_obj_4lxml_5etree__SaxParserTarget *
 [error]       src/lxml/etree.c:137352:105: note: expected ‘struct __pyx_obj_4lxml_5etree_TreeBuilder *’ but argument is of type ‘struct __pyx_obj_4lxml_5etree__SaxParserTarget *’
 [error]       137352 | static int __pyx_f_4lxml_5etree_11TreeBuilder__handleSaxData(struct __pyx_obj_4lxml_5etree_TreeBuilder *__pyx_v_self, PyObject *__pyx_v_data) {
 [error]              |                                                              ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__pyx_pf_4lxml_5etree_11TreeBuilder_6start’:
 [error]       src/lxml/etree.c:138137:67: error: passing argument 1 of ‘__pyx_f_4lxml_5etree_11TreeBuilder__handleSaxStart’ from incompatible pointer type [-Wincompatible-pointer-types]
 [error]       138137 |   __pyx_t_3 = __pyx_f_4lxml_5etree_11TreeBuilder__handleSaxStart(((struct __pyx_obj_4lxml_5etree__SaxParserTarget *)__pyx_v_self), __pyx_v_tag, __pyx_v_attrs, __pyx_v_nsmap); if (unlikely(!__pyx_t_3)) __PYX_ERR(3, 843, __pyx_L1_error)
 [error]              |                                                                  ~^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]              |                                                                   |
 [error]              |                                                                   struct __pyx_obj_4lxml_5etree__SaxParserTarget *
 [error]       src/lxml/etree.c:136951:112: note: expected ‘struct __pyx_obj_4lxml_5etree_TreeBuilder *’ but argument is of type ‘struct __pyx_obj_4lxml_5etree__SaxParserTarget *’
 [error]       136951 | static PyObject *__pyx_f_4lxml_5etree_11TreeBuilder__handleSaxStart(struct __pyx_obj_4lxml_5etree_TreeBuilder *__pyx_v_self, PyObject *__pyx_v_tag, PyObject *__pyx_v_attrib, PyObject *__pyx_v_nsmap) {
 [error]              |                                                                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__pyx_pf_4lxml_5etree_11TreeBuilder_8end’:
 [error]       src/lxml/etree.c:138208:65: error: passing argument 1 of ‘__pyx_f_4lxml_5etree_11TreeBuilder__handleSaxEnd’ from incompatible pointer type [-Wincompatible-pointer-types]
 [error]       138208 |   __pyx_t_1 = __pyx_f_4lxml_5etree_11TreeBuilder__handleSaxEnd(((struct __pyx_obj_4lxml_5etree__SaxParserTarget *)__pyx_v_self), __pyx_v_tag); if (unlikely(!__pyx_t_1)) __PYX_ERR(3, 850, __pyx_L1_error)
 [error]              |                                                                ~^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]              |                                                                 |
 [error]              |                                                                 struct __pyx_obj_4lxml_5etree__SaxParserTarget *
 [error]       src/lxml/etree.c:137251:110: note: expected ‘struct __pyx_obj_4lxml_5etree_TreeBuilder *’ but argument is of type ‘struct __pyx_obj_4lxml_5etree__SaxParserTarget *’
 [error]       137251 | static PyObject *__pyx_f_4lxml_5etree_11TreeBuilder__handleSaxEnd(struct __pyx_obj_4lxml_5etree_TreeBuilder *__pyx_v_self, CYTHON_UNUSED PyObject *__pyx_v_tag) {
 [error]              |                                                                   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__pyx_pf_4lxml_5etree_11TreeBuilder_10pi’:
 [error]       src/lxml/etree.c:138409:64: error: passing argument 1 of ‘__pyx_f_4lxml_5etree_11TreeBuilder__handleSaxPi’ from incompatible pointer type [-Wincompatible-pointer-types]
 [error]       138409 |   __pyx_t_1 = __pyx_f_4lxml_5etree_11TreeBuilder__handleSaxPi(((struct __pyx_obj_4lxml_5etree__SaxParserTarget *)__pyx_v_self), __pyx_v_target, __pyx_v_data); if (unlikely(!__pyx_t_1)) __PYX_ERR(3, 861, __pyx_L1_error)
 [error]              |                                                               ~^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]              |                                                                |
 [error]              |                                                                struct __pyx_obj_4lxml_5etree__SaxParserTarget *
 [error]       src/lxml/etree.c:137401:109: note: expected ‘struct __pyx_obj_4lxml_5etree_TreeBuilder *’ but argument is of type ‘struct __pyx_obj_4lxml_5etree__SaxParserTarget *’
 [error]       137401 | static PyObject *__pyx_f_4lxml_5etree_11TreeBuilder__handleSaxPi(struct __pyx_obj_4lxml_5etree_TreeBuilder *__pyx_v_self, PyObject *__pyx_v_target, PyObject *__pyx_v_data) {
 [error]              |                                                                  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__pyx_pf_4lxml_5etree_11TreeBuilder_12comment’:
 [error]       src/lxml/etree.c:138472:69: error: passing argument 1 of ‘__pyx_f_4lxml_5etree_11TreeBuilder__handleSaxComment’ from incompatible pointer type [-Wincompatible-pointer-types]
 [error]       138472 |   __pyx_t_1 = __pyx_f_4lxml_5etree_11TreeBuilder__handleSaxComment(((struct __pyx_obj_4lxml_5etree__SaxParserTarget *)__pyx_v_self), __pyx_v_comment); if (unlikely(!__pyx_t_1)) __PYX_ERR(3, 869, __pyx_L1_error)
 [error]              |                                                                    ~^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]              |                                                                     |
 [error]              |                                                                     struct __pyx_obj_4lxml_5etree__SaxParserTarget *
 [error]       src/lxml/etree.c:137607:114: note: expected ‘struct __pyx_obj_4lxml_5etree_TreeBuilder *’ but argument is of type ‘struct __pyx_obj_4lxml_5etree__SaxParserTarget *’
 [error]       137607 | static PyObject *__pyx_f_4lxml_5etree_11TreeBuilder__handleSaxComment(struct __pyx_obj_4lxml_5etree_TreeBuilder *__pyx_v_self, PyObject *__pyx_v_comment) {
 [error]              |                                                                       ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__pyx_f_4lxml_5etree_12_BaseContext__set_xpath_context’:
 [error]       src/lxml/etree.c:181971:28: error: assignment to ‘xmlStructuredErrorFunc’ {aka ‘void (*)(void *, const struct _xmlError *)’} from incompatible pointer type ‘void (*)(void *, xmlError *)’ {aka ‘void (*)(void *, struct _xmlError *)’} [-Wincompatible-pointer-types]
 [error]       181971 |   __pyx_v_xpathCtxt->error = __pyx_f_4lxml_5etree__receiveXPathError;
 [error]              |                            ^
 [error]       src/lxml/etree.c:7546:13: note: ‘__pyx_f_4lxml_5etree__receiveXPathError’ declared here
 [error]        7546 | static void __pyx_f_4lxml_5etree__receiveXPathError(void *, xmlError *); /*proto*/
 [error]             |             ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]       /usr/include/libxml2/libxml/xmlerror.h:953:16: note: ‘xmlStructuredErrorFunc’ declared here
 [error]         953 | typedef void (*xmlStructuredErrorFunc) (void *userData, const xmlError *error);
 [error]             |                ^~~~~~~~~~~~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__pyx_pf_4lxml_5etree_4XSLT_18__call__’:
 [error]       src/lxml/etree.c:203486:73: error: passing argument 1 of ‘__pyx_f_4lxml_5etree_12_XSLTContext__copy’ from incompatible pointer type [-Wincompatible-pointer-types]
 [error]       203486 |     __pyx_t_1 = ((PyObject *)__pyx_f_4lxml_5etree_12_XSLTContext__copy(((struct __pyx_obj_4lxml_5etree__BaseContext *)__pyx_v_self->_context))); if (unlikely(!__pyx_t_1)) __PYX_ERR(4, 550, __pyx_L9_error)
 [error]              |                                                                        ~^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]              |                                                                         |
 [error]              |                                                                         struct __pyx_obj_4lxml_5etree__BaseContext *
 [error]       src/lxml/etree.c:201169:138: note: expected ‘struct __pyx_obj_4lxml_5etree__XSLTContext *’ but argument is of type ‘struct __pyx_obj_4lxml_5etree__BaseContext *’
 [error]       201169 | static struct __pyx_obj_4lxml_5etree__BaseContext *__pyx_f_4lxml_5etree_12_XSLTContext__copy(struct __pyx_obj_4lxml_5etree__XSLTContext *__pyx_v_self) {
 [error]              |                                                                                              ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__pyx_f_4lxml_5etree__copyXSLT’:
 [error]       src/lxml/etree.c:205311:71: error: passing argument 1 of ‘__pyx_f_4lxml_5etree_12_XSLTContext__copy’ from incompatible pointer type [-Wincompatible-pointer-types]
 [error]       205311 |   __pyx_t_1 = ((PyObject *)__pyx_f_4lxml_5etree_12_XSLTContext__copy(((struct __pyx_obj_4lxml_5etree__BaseContext *)__pyx_v_stylesheet->_context))); if (unlikely(!__pyx_t_1)) __PYX_ERR(4, 691, __pyx_L1_error)
 [error]              |                                                                      ~^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]              |                                                                       |
 [error]              |                                                                       struct __pyx_obj_4lxml_5etree__BaseContext *
 [error]       src/lxml/etree.c:201169:138: note: expected ‘struct __pyx_obj_4lxml_5etree__XSLTContext *’ but argument is of type ‘struct __pyx_obj_4lxml_5etree__BaseContext *’
 [error]       201169 | static struct __pyx_obj_4lxml_5etree__BaseContext *__pyx_f_4lxml_5etree_12_XSLTContext__copy(struct __pyx_obj_4lxml_5etree__XSLTContext *__pyx_v_self) {
 [error]              |                                                                                              ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__pyx_pf_4lxml_5etree_7RelaxNG_2__init__’:
 [error]       src/lxml/etree.c:219245:60: error: passing argument 2 of ‘xmlRelaxNGSetParserStructuredErrors’ from incompatible pointer type [-Wincompatible-pointer-types]
 [error]       219245 |   xmlRelaxNGSetParserStructuredErrors(__pyx_v_parser_ctxt, __pyx_f_4lxml_5etree__receiveError, ((void *)__pyx_v_self->__pyx_base._error_log));
 [error]              |                                                            ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]              |                                                            |
 [error]              |                                                            void (*)(void *, xmlError *) {aka void (*)(void *, struct _xmlError *)}
 [error]       In file included from src/lxml/etree.c:905:
 [error]       /usr/include/libxml2/libxml/relaxng.h:154:65: note: expected ‘xmlStructuredErrorFunc’ {aka ‘void (*)(void *, const struct _xmlError *)’} but argument is of type ‘void (*)(void *, xmlError *)’ {aka ‘void (*)(void *, struct _xmlError *)’}
 [error]         154 |                                          xmlStructuredErrorFunc serror,
 [error]             |                                          ~~~~~~~~~~~~~~~~~~~~~~~^~~~~~
 [error]       src/lxml/etree.c:49484:13: note: ‘__pyx_f_4lxml_5etree__receiveError’ declared here
 [error]       49484 | static void __pyx_f_4lxml_5etree__receiveError(void *__pyx_v_c_log_handler, xmlError *__pyx_v_error) {
 [error]             |             ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]       /usr/include/libxml2/libxml/xmlerror.h:953:16: note: ‘xmlStructuredErrorFunc’ declared here
 [error]         953 | typedef void (*xmlStructuredErrorFunc) (void *userData, const xmlError *error);
 [error]             |                ^~~~~~~~~~~~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__pyx_pf_4lxml_5etree_7RelaxNG_6__call__’:
 [error]       src/lxml/etree.c:219663:60: error: passing argument 2 of ‘xmlRelaxNGSetValidStructuredErrors’ from incompatible pointer type [-Wincompatible-pointer-types]
 [error]       219663 |     xmlRelaxNGSetValidStructuredErrors(__pyx_v_valid_ctxt, __pyx_f_4lxml_5etree__receiveError, ((void *)__pyx_v_self->__pyx_base._error_log));
 [error]              |                                                            ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]              |                                                            |
 [error]              |                                                            void (*)(void *, xmlError *) {aka void (*)(void *, struct _xmlError *)}
 [error]       /usr/include/libxml2/libxml/relaxng.h:189:66: note: expected ‘xmlStructuredErrorFunc’ {aka ‘void (*)(void *, const struct _xmlError *)’} but argument is of type ‘void (*)(void *, xmlError *)’ {aka ‘void (*)(void *, struct _xmlError *)’}
 [error]         189 |                                           xmlStructuredErrorFunc serror, void *ctx);
 [error]             |                                           ~~~~~~~~~~~~~~~~~~~~~~~^~~~~~
 [error]       src/lxml/etree.c:49484:13: note: ‘__pyx_f_4lxml_5etree__receiveError’ declared here
 [error]       49484 | static void __pyx_f_4lxml_5etree__receiveError(void *__pyx_v_c_log_handler, xmlError *__pyx_v_error) {
 [error]             |             ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]       /usr/include/libxml2/libxml/xmlerror.h:953:16: note: ‘xmlStructuredErrorFunc’ declared here
 [error]         953 | typedef void (*xmlStructuredErrorFunc) (void *userData, const xmlError *error);
 [error]             |                ^~~~~~~~~~~~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__pyx_pf_4lxml_5etree_9XMLSchema_2__init__’:
 [error]       src/lxml/etree.c:220552:59: error: passing argument 2 of ‘xmlSchemaSetParserStructuredErrors’ from incompatible pointer type [-Wincompatible-pointer-types]
 [error]       220552 |   xmlSchemaSetParserStructuredErrors(__pyx_v_parser_ctxt, __pyx_f_4lxml_5etree__receiveError, ((void *)__pyx_v_self->__pyx_base._error_log));
 [error]              |                                                           ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]              |                                                           |
 [error]              |                                                           void (*)(void *, xmlError *) {aka void (*)(void *, struct _xmlError *)}
 [error]       In file included from src/lxml/etree.c:906:
 [error]       /usr/include/libxml2/libxml/xmlschemas.h:151:65: note: expected ‘xmlStructuredErrorFunc’ {aka ‘void (*)(void *, const struct _xmlError *)’} but argument is of type ‘void (*)(void *, xmlError *)’ {aka ‘void (*)(void *, struct _xmlError *)’}
 [error]         151 |                                          xmlStructuredErrorFunc serror,
 [error]             |                                          ~~~~~~~~~~~~~~~~~~~~~~~^~~~~~
 [error]       src/lxml/etree.c:49484:13: note: ‘__pyx_f_4lxml_5etree__receiveError’ declared here
 [error]       49484 | static void __pyx_f_4lxml_5etree__receiveError(void *__pyx_v_c_log_handler, xmlError *__pyx_v_error) {
 [error]             |             ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]       /usr/include/libxml2/libxml/xmlerror.h:953:16: note: ‘xmlStructuredErrorFunc’ declared here
 [error]         953 | typedef void (*xmlStructuredErrorFunc) (void *userData, const xmlError *error);
 [error]             |                ^~~~~~~~~~~~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__pyx_pf_4lxml_5etree_9XMLSchema_6__call__’:
 [error]       src/lxml/etree.c:221095:59: error: passing argument 2 of ‘xmlSchemaSetValidStructuredErrors’ from incompatible pointer type [-Wincompatible-pointer-types]
 [error]       221095 |     xmlSchemaSetValidStructuredErrors(__pyx_v_valid_ctxt, __pyx_f_4lxml_5etree__receiveError, ((void *)__pyx_v_self->__pyx_base._error_log));
 [error]              |                                                           ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]              |                                                           |
 [error]              |                                                           void (*)(void *, xmlError *) {aka void (*)(void *, struct _xmlError *)}
 [error]       /usr/include/libxml2/libxml/xmlschemas.h:184:65: note: expected ‘xmlStructuredErrorFunc’ {aka ‘void (*)(void *, const struct _xmlError *)’} but argument is of type ‘void (*)(void *, xmlError *)’ {aka ‘void (*)(void *, struct _xmlError *)’}
 [error]         184 |                                          xmlStructuredErrorFunc serror,
 [error]             |                                          ~~~~~~~~~~~~~~~~~~~~~~~^~~~~~
 [error]       src/lxml/etree.c:49484:13: note: ‘__pyx_f_4lxml_5etree__receiveError’ declared here
 [error]       49484 | static void __pyx_f_4lxml_5etree__receiveError(void *__pyx_v_c_log_handler, xmlError *__pyx_v_error) {
 [error]             |             ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]       /usr/include/libxml2/libxml/xmlerror.h:953:16: note: ‘xmlStructuredErrorFunc’ declared here
 [error]         953 | typedef void (*xmlStructuredErrorFunc) (void *userData, const xmlError *error);
 [error]             |                ^~~~~~~~~~~~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__pyx_f_4lxml_5etree_30_ParserSchemaValidationContext_connect’:
 [error]       src/lxml/etree.c:221860:66: error: passing argument 2 of ‘xmlSchemaSetValidStructuredErrors’ from incompatible pointer type [-Wincompatible-pointer-types]
 [error]       221860 |     xmlSchemaSetValidStructuredErrors(__pyx_v_self->_valid_ctxt, __pyx_f_4lxml_5etree__receiveError, ((void *)__pyx_v_error_log));
 [error]              |                                                                  ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]              |                                                                  |
 [error]              |                                                                  void (*)(void *, xmlError *) {aka void (*)(void *, struct _xmlError *)}
 [error]       /usr/include/libxml2/libxml/xmlschemas.h:184:65: note: expected ‘xmlStructuredErrorFunc’ {aka ‘void (*)(void *, const struct _xmlError *)’} but argument is of type ‘void (*)(void *, xmlError *)’ {aka ‘void (*)(void *, struct _xmlError *)’}
 [error]         184 |                                          xmlStructuredErrorFunc serror,
 [error]             |                                          ~~~~~~~~~~~~~~~~~~~~~~~^~~~~~
 [error]       src/lxml/etree.c:49484:13: note: ‘__pyx_f_4lxml_5etree__receiveError’ declared here
 [error]       49484 | static void __pyx_f_4lxml_5etree__receiveError(void *__pyx_v_c_log_handler, xmlError *__pyx_v_error) {
 [error]             |             ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]       /usr/include/libxml2/libxml/xmlerror.h:953:16: note: ‘xmlStructuredErrorFunc’ declared here
 [error]         953 | typedef void (*xmlStructuredErrorFunc) (void *userData, const xmlError *error);
 [error]             |                ^~~~~~~~~~~~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__pyx_pf_4lxml_5etree_10Schematron_6__call__’:
 [error]       src/lxml/etree.c:223037:63: error: passing argument 2 of ‘xmlSchematronSetValidStructuredErrors’ from incompatible pointer type [-Wincompatible-pointer-types]
 [error]       223037 |     xmlSchematronSetValidStructuredErrors(__pyx_v_valid_ctxt, __pyx_f_4lxml_5etree__receiveError, ((void *)__pyx_v_self->__pyx_base._error_log));
 [error]              |                                                               ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]              |                                                               |
 [error]              |                                                               void (*)(void *, xmlError *) {aka void (*)(void *, struct _xmlError *)}
 [error]       In file included from src/lxml/etree.c:907:
 [error]       /usr/include/libxml2/libxml/schematron.h:114:66: note: expected ‘xmlStructuredErrorFunc’ {aka ‘void (*)(void *, const struct _xmlError *)’} but argument is of type ‘void (*)(void *, xmlError *)’ {aka ‘void (*)(void *, struct _xmlError *)’}
 [error]         114 |                                           xmlStructuredErrorFunc serror,
 [error]             |                                           ~~~~~~~~~~~~~~~~~~~~~~~^~~~~~
 [error]       src/lxml/etree.c:49484:13: note: ‘__pyx_f_4lxml_5etree__receiveError’ declared here
 [error]       49484 | static void __pyx_f_4lxml_5etree__receiveError(void *__pyx_v_c_log_handler, xmlError *__pyx_v_error) {
 [error]             |             ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]       /usr/include/libxml2/libxml/xmlerror.h:953:16: note: ‘xmlStructuredErrorFunc’ declared here
 [error]         953 | typedef void (*xmlStructuredErrorFunc) (void *userData, const xmlError *error);
 [error]             |                ^~~~~~~~~~~~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__pyx_pymod_exec_etree’:
 [error]       src/lxml/etree.c:6652:38: error: implicit declaration of function ‘_PyDict_SetItem_KnownHash’; did you mean ‘_PyDict_GetItem_KnownHash’? [-Wimplicit-function-declaration]
 [error]        6652 |     (likely(PyDict_CheckExact(ns)) ? _PyDict_SetItem_KnownHash(ns, name, value, ((PyASCIIObject *) name)->hash) : PyObject_SetItem(ns, name, value))
 [error]             |                                      ^~~~~~~~~~~~~~~~~~~~~~~~~
 [error]       src/lxml/etree.c:255376:7: note: in expansion of macro ‘__Pyx_SetNameInClass’
 [error]       255376 |   if (__Pyx_SetNameInClass(__pyx_t_2, __pyx_n_s_getitem, __pyx_t_9) < 0) __PYX_ERR(0, 97, __pyx_L1_error)
 [error]              |       ^~~~~~~~~~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__Pyx_PyUnicode_Join’:
 [error]       src/lxml/etree.c:264446:13: error: implicit declaration of function ‘_PyUnicode_FastCopyCharacters’; did you mean ‘PyUnicode_CopyCharacters’? [-Wimplicit-function-declaration]
 [error]       264446 |             _PyUnicode_FastCopyCharacters(result_uval, char_pos, uval, 0, ulength);
 [error]              |             ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]              |             PyUnicode_CopyCharacters
 [error]       src/lxml/etree.c: In function ‘__Pyx_PyIter_Next2’:
 [error]       src/lxml/etree.c:265182:35: error: ‘_PyObject_NextNotImplemented’ undeclared (first use in this function); did you mean ‘PyObject_HashNotImplemented’?
 [error]       265182 |         if (unlikely(iternext == &_PyObject_NextNotImplemented))
 [error]              |                                   ^~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]       src/lxml/etree.c:1096:43: note: in definition of macro ‘unlikely’
 [error]        1096 |   #define unlikely(x) __builtin_expect(!!(x), 0)
 [error]             |                                           ^
 [error]       src/lxml/etree.c:265182:35: note: each undeclared identifier is reported only once for each function it appears in
 [error]       265182 |         if (unlikely(iternext == &_PyObject_NextNotImplemented))
 [error]              |                                   ^~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]       src/lxml/etree.c:1096:43: note: in definition of macro ‘unlikely’
 [error]        1096 |   #define unlikely(x) __builtin_expect(!!(x), 0)
 [error]             |                                           ^
 [error]       src/lxml/etree.c: In function ‘__Pyx_PyGen_Send’:
 [error]       src/lxml/etree.c:265973:13: error: implicit declaration of function ‘_PyGen_SetStopIterationValue’; did you mean ‘__Pyx_PyGen__FetchStopIterationValue’? [-Wimplicit-function-declaration]
 [error]       265973 |             _PyGen_SetStopIterationValue(result);
 [error]              |             ^~~~~~~~~~~~~~~~~~~~~~~~~~~~
 [error]              |             __Pyx_PyGen__FetchStopIterationValue
 [error]       src/lxml/etree.c: In function ‘__Pyx_Coroutine_AwaitableIterError’:
 [error]       src/lxml/etree.c:267806:5: error: implicit declaration of function ‘_PyErr_FormatFromCause’ [-Wimplicit-function-declaration]
 [error]       267806 |     _PyErr_FormatFromCause(
 [error]              |     ^~~~~~~~~~~~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__Pyx_set_iter_next’:
 [error]       src/lxml/etree.c:268071:19: error: implicit declaration of function ‘_PySet_NextEntry’ [-Wimplicit-function-declaration]
 [error]       268071 |         int ret = _PySet_NextEntry(iter_obj, ppos, value, &hash);
 [error]              |                   ^~~~~~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__Pyx_PyInt_As_int’:
 [error]       src/lxml/etree.c:269066:27: error: too few arguments to function ‘_PyLong_AsByteArray’; expected 6, have 5
 [error]       269066 |                 int ret = _PyLong_AsByteArray((PyLongObject *)v,
 [error]              |                           ^~~~~~~~~~~~~~~~~~~
 [error]       In file included from /usr/include/python3.13/longobject.h:107,
 [error]                        from /usr/include/python3.13/Python.h:82,
 [error]                        from src/lxml/etree.c:96:
 [error]       /usr/include/python3.13/cpython/longobject.h:111:17: note: declared here
 [error]         111 | PyAPI_FUNC(int) _PyLong_AsByteArray(PyLongObject* v,
 [error]             |                 ^~~~~~~~~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__Pyx_PyInt_As_size_t’:
 [error]       src/lxml/etree.c:269375:27: error: too few arguments to function ‘_PyLong_AsByteArray’; expected 6, have 5
 [error]       269375 |                 int ret = _PyLong_AsByteArray((PyLongObject *)v,
 [error]              |                           ^~~~~~~~~~~~~~~~~~~
 [error]       /usr/include/python3.13/cpython/longobject.h:111:17: note: declared here
 [error]         111 | PyAPI_FUNC(int) _PyLong_AsByteArray(PyLongObject* v,
 [error]             |                 ^~~~~~~~~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__Pyx_PyInt_As_unsigned_int’:
 [error]       src/lxml/etree.c:269571:27: error: too few arguments to function ‘_PyLong_AsByteArray’; expected 6, have 5
 [error]       269571 |                 int ret = _PyLong_AsByteArray((PyLongObject *)v,
 [error]              |                           ^~~~~~~~~~~~~~~~~~~
 [error]       /usr/include/python3.13/cpython/longobject.h:111:17: note: declared here
 [error]         111 | PyAPI_FUNC(int) _PyLong_AsByteArray(PyLongObject* v,
 [error]             |                 ^~~~~~~~~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__Pyx_PyInt_As_signed__char’:
 [error]       src/lxml/etree.c:269767:27: error: too few arguments to function ‘_PyLong_AsByteArray’; expected 6, have 5
 [error]       269767 |                 int ret = _PyLong_AsByteArray((PyLongObject *)v,
 [error]              |                           ^~~~~~~~~~~~~~~~~~~
 [error]       /usr/include/python3.13/cpython/longobject.h:111:17: note: declared here
 [error]         111 | PyAPI_FUNC(int) _PyLong_AsByteArray(PyLongObject* v,
 [error]             |                 ^~~~~~~~~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__Pyx_PyInt_As_unsigned_short’:
 [error]       src/lxml/etree.c:270001:27: error: too few arguments to function ‘_PyLong_AsByteArray’; expected 6, have 5
 [error]       270001 |                 int ret = _PyLong_AsByteArray((PyLongObject *)v,
 [error]              |                           ^~~~~~~~~~~~~~~~~~~
 [error]       /usr/include/python3.13/cpython/longobject.h:111:17: note: declared here
 [error]         111 | PyAPI_FUNC(int) _PyLong_AsByteArray(PyLongObject* v,
 [error]             |                 ^~~~~~~~~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__Pyx_PyInt_As_xmlChar’:
 [error]       src/lxml/etree.c:270235:27: error: too few arguments to function ‘_PyLong_AsByteArray’; expected 6, have 5
 [error]       270235 |                 int ret = _PyLong_AsByteArray((PyLongObject *)v,
 [error]              |                           ^~~~~~~~~~~~~~~~~~~
 [error]       /usr/include/python3.13/cpython/longobject.h:111:17: note: declared here
 [error]         111 | PyAPI_FUNC(int) _PyLong_AsByteArray(PyLongObject* v,
 [error]             |                 ^~~~~~~~~~~~~~~~~~~
 [error]       src/lxml/etree.c: In function ‘__Pyx_PyInt_As_long’:
 [error]       src/lxml/etree.c:270438:27: error: too few arguments to function ‘_PyLong_AsByteArray’; expected 6, have 5
 [error]       270438 |                 int ret = _PyLong_AsByteArray((PyLongObject *)v,
 [error]              |                           ^~~~~~~~~~~~~~~~~~~
 [error]       /usr/include/python3.13/cpython/longobject.h:111:17: note: declared here
 [error]         111 | PyAPI_FUNC(int) _PyLong_AsByteArray(PyLongObject* v,
 [error]             |                 ^~~~~~~~~~~~~~~~~~~
 [error]       Compile failed: command '/usr/bin/gcc' failed with exit code 1
 [error]       creating tmp
 [error]       cc -I/usr/include/libxml2 -I/usr/include/libxml2 -c /tmp/xmlXPathInithkbvx1aj.c -o tmp/xmlXPathInithkbvx1aj.o
 [error]       /tmp/xmlXPathInithkbvx1aj.c: In function ‘main’:
 [error]       /tmp/xmlXPathInithkbvx1aj.c:3:5: warning: ‘xmlXPathInit’ is deprecated: See https://gnome.pages.gitlab.gnome.org/libxml2/html/deprecated.html [-Wdeprecated-declarations]
 [error]           3 |     xmlXPathInit();
 [error]             |     ^~~~~~~~~~~~
 [error]       In file included from /tmp/xmlXPathInithkbvx1aj.c:1:
 [error]       /usr/include/libxml2/libxml/xpath.h:561:21: note: declared here
 [error]         561 |                     xmlXPathInit                (void);
 [error]             |                     ^~~~~~~~~~~~
 [error]       cc tmp/xmlXPathInithkbvx1aj.o -lxml2 -o a.out
 [error]       error: command '/usr/bin/gcc' failed with exit code 1
 [error]       [end of output]
 [error]   
 [error]   note: This error originates from a subprocess, and is likely not a problem with pip.
 [error]   ERROR: Failed building wheel for lxml
 [error] 
 [error] [notice] A new release of pip is available: 24.3.1 -> 25.3
 [error] [notice] To update, run: python -m pip install --upgrade pip
 [error] ERROR: ERROR: Failed to build installable wheels for some pyproject.toml based projects (lxml)
  [exit] 1
```
解决，通过pcman安装
```shell
sudo pacman -S python-lxml
```
安装后，最新版本大于我的4.9.7，然后把requirements.txt中lxml的版本修改为>=4.9.7即可

安装过程种报timeout，添加国内源
```shell
python3 -m pip install "lxml>=4.9.4" -i https://pypi.tuna.tsinghua.edu.cn/simple --timeout 120 --retries 10 --no-cache-dir
```

安装成功


