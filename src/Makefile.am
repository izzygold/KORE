DIST_SUBDIRS = secp256k1 univalue
LIBS+=  -lssl -lcrypto -lcurl
AM_LDFLAGS = $(PTHREAD_CFLAGS) $(LIBTOOL_LDFLAGS) $(HARDENED_LDFLAGS)
AM_CXXFLAGS = $(HARDENED_CXXFLAGS)
AM_CPPFLAGS = $(HARDENED_CPPFLAGS)
EXTRA_LIBRARIES =

BITCOIN_CONFIG_INCLUDES=-I$(builddir)/config
BITCOIN_INCLUDES=-I$(builddir) -I$(builddir)/obj $(BOOST_CPPFLAGS) $(LEVELDB_CPPFLAGS) $(CRYPTO_CFLAGS) $(SSL_CFLAGS)

BITCOIN_INCLUDES += -I$(srcdir)/secp256k1/include
BITCOIN_INCLUDES += -I$(srcdir)/univalue/include

LIBBITCOIN_SERVER=libkore_server.a
LIBBITCOIN_WALLET=libkore_wallet.a
LIBBITCOIN_COMMON=libkore_common.a
LIBBITCOIN_CLI=libkore_cli.a
LIBBITCOIN_UTIL=libkore_util.a
LIBBITCOIN_CRYPTO=crypto/libkore_crypto.a
LIBBITCOINQT=qt/libkoreqt.a
LIBSECP256K1=secp256k1/libsecp256k1.la
LIBUNIVALUE=univalue/libunivalue.la

$(LIBSECP256K1): $(wildcard secp256k1/src/*) $(wildcard secp256k1/include/*)
	$(AM_V_at)$(MAKE) $(AM_MAKEFLAGS) -C $(@D) $(@F)

$(LIBUNIVALUE): $(wildcard univalue/lib/*) $(wildcard univalue/include/*)
	$(AM_V_at)$(MAKE) $(AM_MAKEFLAGS) -C $(@D) $(@F)

# Make is not made aware of per-object dependencies to avoid limiting building parallelization
# But to build the less dependent modules first, we manually select their order here:
EXTRA_LIBRARIES += \
  crypto/libkore_crypto.a \
  libkore_util.a \
  libkore_common.a \
  libkore_server.a \
  libkore_cli.a
if ENABLE_WALLET
BITCOIN_INCLUDES += $(BDB_CPPFLAGS)
EXTRA_LIBRARIES += libkore_wallet.a
endif
if ENABLE_ZMQ
EXTRA_LIBRARIES += libkore_zmq.a
endif

if BUILD_BITCOIN_LIBS
lib_LTLIBRARIES = libkoreconsensus.la
LIBBITCOIN_CONSENSUS=libkoreconsensus.la
else
LIBBITCOIN_CONSENSUS=
endif

bin_PROGRAMS =
TESTS =
BENCHMARKS =

if BUILD_BITCOIND
  bin_PROGRAMS += kored
endif

if BUILD_BITCOIN_UTILS
  bin_PROGRAMS += kore-cli kore-tx
endif

.PHONY: FORCE check-symbols check-security
# kore core #
BITCOIN_CORE_H = \
  activemasternode.h \
  addrman.h \
  alert.h \
  amount.h \
  arith_uint256.h \
  axiom.h \
  base58.h \
  bloom.h \
  chain.h \
  chainparams.h \
  chainparamsbase.h \
  chainparamsseeds.h \
  checkpoints.h \
  checkqueue.h \
  clientversion.h \
  coincontrol.h \
  coins.h \
  compat.h \
  compat/byteswap.h \
  compat/endian.h \
  compat/sanity.h \
  compressor.h \
  consensus/consensus.h \
  consensus/merkle.h \
  consensus/params.h \
  consensus/validation.h \
  core_io.h \
  core_memusage.h \
  obfuscation.h \
  obfuscation-relay.h \
  hash.h \
  httprpc.h \
  httpserver.h \
  init.h \
  swifttx.h \
  key.h \
  keystore.h \
  dbwrapper.h \
  limitedmap.h \
  main.h \
  masternode.h \
  masternode-payments.h \
  masternode-budget.h \
  masternode-sync.h \
  masternodeman.h \
  masternodeconfig.h \
  memusage.h \
  merkleblock.h \
  miner.h \
  momentum.h \
  net.h \
  netbase.h \
  noui.h \
  policy/fees.h \
  policy/policy.h \
  policy/rbf.h \
  pos.h \
  pow.h \
  prevector.h \
  primitives/block.h \
  primitives/transaction.h \
  protocol.h \
  pubkey.h \
  random.h \
  reverselock.h \
  rpc/client.h \
  rpc/protocol.h \
  rpc/server.h \
  scheduler.h \
  script/interpreter.h \
  script/script.h \
  script/script_error.h \
  script/sigcache.h \
  script/sign.h \
  script/standard.h \
  semiOrderedMap.h \
  serialize.h \
  spork.h \
  streams.h \
  support/allocators/secure.h \
  support/allocators/zeroafterfree.h \
  support/cleanse.h \
  support/pagelocker.h \
  sync.h \
  threadsafety.h \
  timedata.h \
  tinyformat.h \
  torcontrol.h \
  txdb.h \
  txmempool.h \
  ui_interface.h \
  uint256.h \
  undo.h \
  util.h \
  utilmoneystr.h \
  utilstrencodings.h \
  utiltime.h \
  validationinterface.h \
  version.h \
  versionbits.h \
  wallet/crypter.h \
  wallet/db.h \
  wallet/wallet.h \
  wallet/wallet_ismine.h \
  wallet/walletdb.h \
  zmq/zmqabstractnotifier.h \
  zmq/zmqconfig.h\
  zmq/zmqnotificationinterface.h \
  zmq/zmqpublishnotifier.h \
  tor/ht.h \
  tor/or.h \
  tor/aes.h \
  tor/dns.h \
  tor/geoip.h \
  tor/onion.h \
  tor/relay.h \
  tor/config.h \
  tor/crypto.h \
  tor/di_ops.h \
  tor/ntmain.h \
  tor/router.h \
  tor/status.h \
  tor/torint.h \
  tor/torlog.h \
  tor/tortls.h \
  tor/address.h \
  tor/buffers.h \
  tor/channel.h \
  tor/command.h \
  tor/control.h \
  tor/dirserv.h \
  tor/dirvote.h \
  tor/dnsserv.h \
  tor/fp_pair.h \
  tor/memarea.h \
  tor/mempool.h \
  tor/procmon.h \
  tor/reasons.h \
  tor/rendmid.h \
  tor/rephist.h \
  tor/sandbox.h \
  tor/torgzip.h \
  tor/eventdns.h \
  tor/kore.h \
  tor/nodelist.h \
  tor/orconfig.h \
  tor/policies.h \
  tor/tinytest.h \
  tor/tor_util.h \
  tor/backtrace.h \
  tor/confparse.h \
  tor/container.h \
  tor/cpuworker.h \
  tor/directory.h \
  tor/hibernate.h \
  tor/microdesc.h \
  tor/onion_tap.h \
  tor/routerset.h \
  tor/statefile.h \
  tor/tor_queue.h \
  tor/addressmap.h \
  tor/channeltls.h \
  tor/circuitmux.h \
  tor/circuituse.h \
  tor/connection.h \
  tor/entrynodes.h \
  tor/ext_orport.h \
  tor/onion_fast.h \
  tor/onion_main.h \
  tor/onion_ntor.h \
  tor/rendclient.h \
  tor/rendcommon.h \
  tor/routerlist.h \
  tor/tor_compat.h \
  tor/transports.h \
  tor/circuitlist.h \
  tor/rendservice.h \
  tor/replaycache.h \
  tor/routerparse.h \
  tor/testsupport.h \
  tor/circpathbias.h \
  tor/circuitbuild.h \
  tor/circuitstats.h \
  tor/eventdns_tor.h \
  tor/connection_or.h \
  tor/networkstatus.h \
  tor/orconfig_apple.h \
  tor/orconfig_linux.h \
  tor/orconfig_win32.h \
  tor/circuitmux_ewma.h \
  tor/compat_libevent.h \
  tor/connection_edge.h \
  tor/tinytest_macros.h \
  tor/crypto_curve25519.h 

obj/build.h: FORCE
	@$(MKDIR_P) $(builddir)/obj
	@$(top_srcdir)/share/genbuild.sh $(abs_top_builddir)/src/obj/build.h \
	  $(abs_top_srcdir)
libkore_util_a-clientversion.$(OBJEXT): obj/build.h

# server: shared between kored and kore-qt
libkore_server_a_CPPFLAGS = -fPIC $(AM_CPPFLAGS) $(BITCOIN_INCLUDES) $(MINIUPNPC_CPPFLAGS) $(EVENT_CFLAGS) $(EVENT_PTHREADS_CFLAGS)
libkore_server_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
LIBS += -lz -levent -levent_core
libkore_server_a_SOURCES = \
  addrman.cpp \
  alert.cpp \
  bloom.cpp \
  chain.cpp \
  checkpoints.cpp \
  httprpc.cpp \
  httpserver.cpp \
  init.cpp \
  dbwrapper.cpp \
  main.cpp \
  merkleblock.cpp \
  miner.cpp \
  net.cpp \
  noui.cpp \
  policy/fees.cpp \
  policy/policy.cpp \
  pos.cpp \
  pow.cpp \
  rest.cpp \
  rpc/blockchain.cpp \
  rpc/masternode.cpp \
  rpc/masternode-budget.cpp \
  rpc/mining.cpp \
  rpc/misc.cpp \
  rpc/net.cpp \
  rpc/rawtransaction.cpp \
  rpc/server.cpp \
  script/sigcache.cpp \
  timedata.cpp \
  tor/address.c \
  tor/addressmap.c \
  tor/aes.c \
  tor/backtrace.c \
  tor/buffers.c \
  tor/channel.c \
  tor/channeltls.c \
  tor/circpathbias.c \
  tor/circuitbuild.c \
  tor/circuitlist.c \
  tor/circuitmux.c \
  tor/circuitmux_ewma.c \
  tor/circuitstats.c \
  tor/circuituse.c \
  tor/command.c \
  tor/compat.c \
  tor/compat_libevent.c \
  tor/config.c \
  tor/config_codedigest.c \
  tor/confparse.c \
  tor/connection.c \
  tor/connection_edge.c \
  tor/connection_or.c \
  tor/container.c \
  tor/control.c \
  tor/cpuworker.c \
  tor/crypto.c \
  tor/crypto_curve25519.c \
  tor/crypto_format.c \
  tor/curve25519-donna.c \
  tor/di_ops.c \
  tor/directory.c \
  tor/dirserv.c \
  tor/dirvote.c \
  tor/dns.c \
  tor/dnsserv.c \
  tor/entrynodes.c \
  tor/ext_orport.c \
  tor/fp_pair.c \
  tor/geoip.c \
  tor/hibernate.c \
  tor/log.c \
  tor/memarea.c \
  tor/mempool.c \
  tor/microdesc.c \
  tor/networkstatus.c \
  tor/nodelist.c \
  tor/onion.c \
  tor/onion_fast.c \
  tor/onion_main.c \
  tor/onion_ntor.c \
  tor/onion_tap.c \
  tor/policies.c \
  tor/kore.cpp \
  tor/procmon.c \
  tor/reasons.c \
  tor/relay.c \
  tor/rendclient.c \
  tor/rendcommon.c \
  tor/rendmid.c \
  tor/rendservice.c \
  tor/rephist.c \
  tor/replaycache.c \
  tor/router.c \
  tor/routerlist.c \
  tor/routerparse.c \
  tor/routerset.c \
  tor/sandbox.c \
  tor/statefile.c \
  tor/status.c \
  tor/strlcat.c \
  tor/strlcpy.c \
  tor/tor_util.c \
  tor/torgzip.c \
  tor/tortls.c \
  tor/transports.c \
  tor/util_codedigest.c \
  tor/kore.cpp \
  torcontrol.cpp \
  txdb.cpp \
  txmempool.cpp \
  validationinterface.cpp \
  versionbits.cpp \
  $(BITCOIN_CORE_H)

if ENABLE_ZMQ
LIBBITCOIN_ZMQ=libkore_zmq.a

libkore_zmq_a_CPPFLAGS = $(BITCOIN_INCLUDES) $(ZMQ_CFLAGS)
libkore_zmq_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libkore_zmq_a_SOURCES = \
  zmq/zmqabstractnotifier.cpp \
  zmq/zmqnotificationinterface.cpp \
  zmq/zmqpublishnotifier.cpp
endif


# wallet: shared between kored and kore-qt, but only linked
# when wallet enabled
libkore_wallet_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
libkore_wallet_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libkore_wallet_a_SOURCES = \
  wallet/crypter.cpp \
  activemasternode.cpp \
  obfuscation.cpp \
  obfuscation-relay.cpp \
  swifttx.cpp \
  masternode.cpp \
  masternode-budget.cpp \
  masternode-payments.cpp \
  masternode-sync.cpp \
  masternodeconfig.cpp \
  masternodeman.cpp \
  spork.cpp \
  wallet/db.cpp \
  wallet/rpcdump.cpp \
  wallet/rpcwallet.cpp \
  wallet/wallet.cpp \
  wallet/wallet_ismine.cpp \
  wallet/walletdb.cpp \
  policy/rbf.cpp \
  $(BITCOIN_CORE_H)

# crypto primitives library
crypto_libkore_crypto_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_CONFIG_INCLUDES)
crypto_libkore_crypto_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
crypto_libkore_crypto_a_SOURCES = \
  crypto/common.h \
  crypto/hmac_sha256.cpp \
  crypto/hmac_sha256.h \
  crypto/hmac_sha512.cpp \
  crypto/hmac_sha512.h \
  crypto/ripemd160.cpp \
  crypto/ripemd160.h \
  crypto/sha1.cpp \
  crypto/sha1.h \
  crypto/sha256.cpp \
  crypto/sha256.h \
  crypto/sha512.cpp \
  crypto/sha512.h \
  crypto/sph_shabal.cpp \
  crypto/sph_shabal.h

# common: shared between kored, and kore-qt and non-server tools
libkore_common_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
libkore_common_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libkore_common_a_SOURCES = \
  amount.cpp \
  axiom.cpp \
  arith_uint256.cpp \
  base58.cpp \
  chainparams.cpp \
  coins.cpp \
  compressor.cpp \
  momentum.cpp \
  consensus/merkle.cpp \
  core_read.cpp \
  core_write.cpp \
  hash.cpp \
  key.cpp \
  keystore.cpp \
  netbase.cpp \
  primitives/block.cpp \
  primitives/transaction.cpp \
  protocol.cpp \
  pubkey.cpp \
  scheduler.cpp \
  script/interpreter.cpp \
  script/script.cpp \
  script/script_error.cpp \
  script/sign.cpp \
  script/standard.cpp \
  $(BITCOIN_CORE_H)

# util: shared between all executables.
# This library *must* be included to make sure that the glibc
# backward-compatibility objects and their sanity checks are linked.
libkore_util_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
libkore_util_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libkore_util_a_SOURCES = \
  support/pagelocker.cpp \
  chainparamsbase.cpp \
  clientversion.cpp \
  compat/glibc_sanity.cpp \
  compat/glibcxx_sanity.cpp \
  compat/strnlen.cpp \
  random.cpp \
  rpc/protocol.cpp \
  support/cleanse.cpp \
  sync.cpp \
  uint256.cpp \
  util.cpp \
  utilmoneystr.cpp \
  utilstrencodings.cpp \
  utiltime.cpp \
  $(BITCOIN_CORE_H)

if GLIBC_BACK_COMPAT
libkore_util_a_SOURCES += compat/glibc_compat.cpp
endif

# cli: shared between kore-cli and kore-qt
libkore_cli_a_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
libkore_cli_a_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
libkore_cli_a_SOURCES = \
  rpc/client.cpp \
  $(BITCOIN_CORE_H)

nodist_libkore_util_a_SOURCES = $(srcdir)/obj/build.h
#

# kored binary #
kored_SOURCES = kored.cpp
kored_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
kored_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
kored_LDFLAGS = $(RELDFLAGS) $(AM_LDFLAGS) $(LIBTOOL_APP_LDFLAGS)

if TARGET_WINDOWS
kored_SOURCES += kored-res.rc
endif

kored_LDADD = \
  $(LIBBITCOIN_SERVER) \
  $(LIBBITCOIN_COMMON) \
  $(LIBUNIVALUE) \
  $(LIBBITCOIN_UTIL) \
  $(LIBBITCOIN_CRYPTO) \
  $(LIBLEVELDB) \
  $(LIBMEMENV) \
  $(LIBSECP256K1)

if ENABLE_ZMQ
kored_LDADD += $(LIBBITCOIN_ZMQ) $(ZMQ_LIBS)
endif

if ENABLE_WALLET
kored_LDADD += libkore_wallet.a
endif

kored_LDADD += $(BOOST_LIBS) $(BDB_LIBS) $(SSL_LIBS) $(CRYPTO_LIBS) $(MINIUPNPC_LIBS) $(EVENT_PTHREADS_LIBS) $(EVENT_LIBS)

# kore-cli binary #
kore_cli_SOURCES = kore-cli.cpp
kore_cli_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES) $(EVENT_CFLAGS)
kore_cli_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
kore_cli_LDFLAGS = $(RELDFLAGS) $(AM_LDFLAGS) $(LIBTOOL_APP_LDFLAGS)

if TARGET_WINDOWS
kore_cli_SOURCES += kore-cli-res.rc
endif

kore_cli_LDADD = \
  $(LIBBITCOIN_CLI) \
  $(LIBUNIVALUE) \
  $(LIBBITCOIN_UTIL)

kore_cli_LDADD += $(BOOST_LIBS) $(SSL_LIBS) $(CRYPTO_LIBS) $(EVENT_LIBS)
#

# kore-tx binary #
kore_tx_SOURCES = kore-tx.cpp
kore_tx_CPPFLAGS = $(AM_CPPFLAGS) $(BITCOIN_INCLUDES)
kore_tx_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)
kore_tx_LDFLAGS = $(RELDFLAGS) $(AM_LDFLAGS) $(LIBTOOL_APP_LDFLAGS)

if TARGET_WINDOWS
kore_tx_SOURCES += kore-tx-res.rc
endif

kore_tx_LDADD = \
  $(LIBUNIVALUE) \
  $(LIBBITCOIN_COMMON) \
  $(LIBBITCOIN_UTIL) \
  $(LIBBITCOIN_CRYPTO) \
  $(LIBSECP256K1)

kore_tx_LDADD += $(BOOST_LIBS) $(CRYPTO_LIBS)
#

# koreconsensus library #
if BUILD_BITCOIN_LIBS
include_HEADERS = script/koreconsensus.h
libkoreconsensus_la_SOURCES = \
  crypto/hmac_sha512.cpp \
  crypto/ripemd160.cpp \
  crypto/sha1.cpp \
  crypto/sha256.cpp \
  crypto/sha512.cpp \
  hash.cpp \
  primitives/transaction.cpp \
  pubkey.cpp \
  script/koreconsensus.cpp \
  script/interpreter.cpp \
  script/script.cpp \
  uint256.cpp \
  utilstrencodings.cpp

if GLIBC_BACK_COMPAT
  libkoreconsensus_la_SOURCES += compat/glibc_compat.cpp
endif

libkoreconsensus_la_LDFLAGS = $(AM_LDFLAGS) -no-undefined $(RELDFLAGS)
libkoreconsensus_la_LIBADD = $(LIBSECP256K1)
libkoreconsensus_la_CPPFLAGS = $(AM_CPPFLAGS) -I$(builddir)/obj -I$(srcdir)/secp256k1/include -DBUILD_BITCOIN_INTERNAL
libkoreconsensus_la_CXXFLAGS = $(AM_CXXFLAGS) $(PIE_FLAGS)

endif
#

CLEANFILES = leveldb/libleveldb.a leveldb/libmemenv.a
CLEANFILES += *.gcda *.gcno
CLEANFILES += compat/*.gcda compat/*.gcno
CLEANFILES += consensus/*.gcda consensus/*.gcno
CLEANFILES += crypto/*.gcda crypto/*.gcno
CLEANFILES += policy/*.gcda policy/*.gcno
CLEANFILES += primitives/*.gcda primitives/*.gcno
CLEANFILES += script/*.gcda script/*.gcno
CLEANFILES += support/*.gcda support/*.gcno
CLEANFILES += univalue/*.gcda univalue/*.gcno
CLEANFILES += wallet/*.gcda wallet/*.gcno
CLEANFILES += wallet/test/*.gcda wallet/test/*.gcno
CLEANFILES += zmq/*.gcda zmq/*.gcno

DISTCLEANFILES = obj/build.h

EXTRA_DIST = leveldb

clean-local:
	-$(MAKE) -C leveldb clean
	-$(MAKE) -C secp256k1 clean
	-$(MAKE) -C univalue clean
	-rm -f leveldb/*/*.gcda leveldb/*/*.gcno leveldb/helpers/memenv/*.gcda leveldb/helpers/memenv/*.gcno
	-rm -f config.h

.rc.o:
	@test -f $(WINDRES)
	$(AM_V_GEN) $(WINDRES) -DWINDRES_PREPROC -i $< -o $@

.mm.o:
	$(AM_V_CXX) $(OBJCXX) $(DEFS) $(DEFAULT_INCLUDES) $(INCLUDES) $(AM_CPPFLAGS) \
	  $(CPPFLAGS) $(AM_CXXFLAGS) $(QT_INCLUDES) $(AM_CXXFLAGS) $(PIE_FLAGS) $(CXXFLAGS) -c -o $@ $<

check-symbols: $(bin_PROGRAMS)
if GLIBC_BACK_COMPAT
	@echo "Checking glibc back compat..."
	$(AM_V_at) READELF=$(READELF) CPPFILT=$(CPPFILT) $(top_srcdir)/contrib/devtools/symbol-check.py < $(bin_PROGRAMS)
endif

check-security: $(bin_PROGRAMS)
if HARDEN
	@echo "Checking binary security..."
	$(AM_V_at) READELF=$(READELF) OBJDUMP=$(OBJDUMP) $(top_srcdir)/contrib/devtools/security-check.py < $(bin_PROGRAMS)
endif

%.pb.cc %.pb.h: %.proto
	@test -f $(PROTOC)
	$(AM_V_GEN) $(PROTOC) --cpp_out=$(@D) --proto_path=$(abspath $(<D) $<)

if EMBEDDED_LEVELDB
include Makefile.leveldb.include
endif

if ENABLE_TESTS
include Makefile.test.include
endif

if ENABLE_BENCH
include Makefile.bench.include
endif

if ENABLE_QT
include Makefile.qt.include
endif

if ENABLE_QT_TESTS
include Makefile.qttest.include
endif
