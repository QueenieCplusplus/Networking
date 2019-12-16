# LDAP_concept
DS, AD, Open Directory, LDAP 

# Abstract: table of contents

* Active Directory for Microsoft

* Open Directory for Apple

* Directory Service 

* X.500

* LDAP Client 

* Special filename extension

* DS Feautues 

* Duplica & Deploy in DS

* Other Application

# AD, Active Directory (used in Microsoft)

It is originated from DS, Directory Service, an organized and ACL-functional system with storage. One directory maps itself to key/val pair, maybe the name and attribute.

And this system is aim at providing service to File System.

# OD, Open Directory (used in Apple)

Apple Open Directory is a fork of OpenLDAP.

A lightweight DS software that store info about user and resource (ACL).

p.s.

With the release of osX 10.5, Apple chose to move away from using the Netinfo directory service which had been used by default for all local accounts and groups in every release of osX 10.0 ~ 10.4. Mac OS X 10.5.

Local accounts are now registered in the Local Plugin, whic

# DS Schema, Directory Service

Below Phone Directory, DNS, FS, and so on are Object, and the info about the Object is stored as Attribute. User with certificates will be allowed to access to the Object and to r/w the Attribute.

If we want to design a complicate directory service, the info (or called attributes hereby) may include: UserID, SSL certificate, Devices, Apps Config, those authority is setup to access the Object to R/W its Info.

* Phone Directory: Name, mobile phone number 

* DNS: domain name, IP address 

* OS: printer, users accounts whitelist 

* FS: directory node name, child node (can be sub-directory or files)

# LDAP

Key is entry, value is attribute, it consists of a k/v pair as one schema.

http://ldapjs.org

# Create LDAP Client

How to create AD?

To create AD using ADSI, Active Directory Service Interface. Developer can create AD by connecting & accessing ADSI, we can think this interface is a DB, it has schema, and can execute CRUD operations.

By using freeware LDAP client tool software to create LDAP:

* OpenLDAP (cross-platform) 

* OpenDJ (cross-platform) 

* Apache Directory Server/Studio(cross-platform)

* Active Directory Explorer (micosoft)

* LDAP Admin(micosoft)

* NetTools(micosoft) 

Links: https://en.wikipedia.org/wiki/List_of_LDAP_software

# LDAP project

https://github.com/nodejs2019/tryLDAP

# .ldif (filename extension)

.ldif is LDAP filename extension.

https://github.com/nodejs2019/forked_OpenLDAP/blob/master/ldif/directory.ldif

The LDAP Data Interchange Format (LDIF) is a standard plain text data interchange format for representing LDAP (Lightweight Directory Access Protocol) directory content and update requests.

LDIF conveys directory content as a set of records, one record for each object (or entry). It also represents update requests, such as (CRUD) Add, Modify, Delete, and Rename, as a set of records, one record for each update request.

# DS features

* IAM

* Certificate

# Duplica & Deploy in DS

* Duplica is for Load Balance.（平衡負載）

* Deploy is for Decentralized-purposed. (去中心化)

# Other Application

* SAP solution Manager 

* IBM Tivoli Directory Server 

* Apache Directory Server

# Key Word

ldif

Aliases

Attributes by OID

Extensible matching

# OID, Object Identifiers

Object Identifiers or OIDs are an identifier mechanism standardized by the International Telecommunications Union (ITU) and ISO/IEC for naming any object, concept, or "thing" with a globally unambiguous persistent name.

# Get Started

    $npm install ldapjs
    
# Code (real LDAP project)

https://github.com/QuinoaPy/forked_node-ldapjs

# Code (../lib/*)

attribute.js
dn.js
filter
message
url.js
server.js
client
protocol.js
search//persistent_search.js

# Modules (package.json)

main : lib/indexjs

# npm install (dependencies)

asn1

assert-plus

once

verror

vasync

ldap-filter

dashdash

backoff

assert-plus

asn1

bunyan

# index.js

        module.exports = {
          Client: client.Client,
          createClient: client.createClient,

          Server: Server,
          createServer: function (options) {
            if (options === undefined)
              options = {};

            if (typeof (options) !== 'object')
              throw new TypeError('options (object) required');

            if (!options.log) {
              options.log = new Logger({
                name: 'ldapjs',
                component: 'client',
                stream: process.stderr
              });
            }

            return new Server(options);
          },

          Attribute: Attribute,
          Change: Change,

          dn: dn,
          DN: dn.DN,
          RDN: dn.RDN,
          parseDN: dn.parse,

          persistentSearch: persistentSearch,
          PersistentSearchCache: persistentSearch.PersistentSearchCache,

          filters: filters,
          parseFilter: filters.parseString,

          url: url,
          parseURL: url.parse
        };


        ///--- Export all the childrenz

        var k;

        for (k in Protocol) {
          if (Protocol.hasOwnProperty(k))
            module.exports[k] = Protocol[k];
        }

        for (k in messages) {
          if (messages.hasOwnProperty(k))
            module.exports[k] = messages[k];
        }

        for (k in controls) {
          if (controls.hasOwnProperty(k))
            module.exports[k] = controls[k];
        }

        for (k in filters) {
          if (filters.hasOwnProperty(k)) {
            if (k !== 'parse' && k !== 'parseString')
              module.exports[k] = filters[k];
          }
        }

        for (k in errors) {
          if (errors.hasOwnProperty(k)) {
            module.exports[k] = errors[k];
          }
        }
       
 # search.js
 
    var assert = require('assert-plus');
    var util = require('util');

    var asn1 = require('asn1');

    var LDAPMessage = require('./message');
    var Attribute = require('../attribute');
    var Protocol = require('../protocol');
    var lassert = require('../assert');


    ///--- Globals

    var BerWriter = asn1.BerWriter;


    ///--- API

    function SearchEntry(options) {
      options = options || {};
      assert.object(options);
      lassert.optionalStringDN(options.objectName);

      options.protocolOp = Protocol.LDAP_REP_SEARCH_ENTRY;
      LDAPMessage.call(this, options);

      this.objectName = options.objectName || null;
      this.setAttributes(options.attributes || []);
    }
    util.inherits(SearchEntry, LDAPMessage);
    Object.defineProperties(SearchEntry.prototype, {
      type: {
        get: function getType() { return 'SearchEntry'; },
        configurable: false
      },
      _dn: {
        get: function getDN() { return this.objectName; },
        configurable: false
      },
      object: {
        get: function getObject() {
          var obj = {
            dn: this.dn.toString(),
            controls: []
          };
          this.attributes.forEach(function (a) {
            if (a.vals && a.vals.length) {
              if (a.vals.length > 1) {
                obj[a.type] = a.vals.slice();
              } else {
                obj[a.type] = a.vals[0];
              }
            } else {
              obj[a.type] = [];
            }
          });
          this.controls.forEach(function (element, index, array) {
            obj.controls.push(element.json);
          });
          return obj;
        },
        configurable: false
      },
      raw: {
        get: function getRaw() {
          var obj = {
            dn: this.dn.toString(),
            controls: []
          };

          this.attributes.forEach(function (a) {
            if (a.buffers && a.buffers.length) {
              if (a.buffers.length > 1) {
                obj[a.type] = a.buffers.slice();
              } else {
                obj[a.type] = a.buffers[0];
              }
            } else {
              obj[a.type] = [];
            }
              });
          this.controls.forEach(function (element, index, array) {
            obj.controls.push(element.json);
          });
          return obj;
        },
        configurable: false
      }
    });

    SearchEntry.prototype.addAttribute = function (attr) {
      if (!attr || typeof (attr) !== 'object')
        throw new TypeError('attr (attribute) required');

      this.attributes.push(attr);
    };

    SearchEntry.prototype.toObject = function () {
      return this.object;
    };

    SearchEntry.prototype.fromObject = function (obj) {
      if (typeof (obj) !== 'object')
        throw new TypeError('object required');

      var self = this;
      if (obj.controls)
        this.controls = obj.controls;

      if (obj.attributes)
        obj = obj.attributes;
      this.attributes = [];

      Object.keys(obj).forEach(function (k) {
        self.attributes.push(new Attribute({type: k, vals: obj[k]}));
      });

      return true;
    };

    SearchEntry.prototype.setAttributes = function (obj) {
      if (typeof (obj) !== 'object')
        throw new TypeError('object required');

      if (Array.isArray(obj)) {
        obj.forEach(function (a) {
          if (!Attribute.isAttribute(a))
            throw new TypeError('entry must be an Array of Attributes');
        });
        this.attributes = obj;
      } else {
        var self = this;

        self.attributes = [];
        Object.keys(obj).forEach(function (k) {
          var attr = new Attribute({type: k});
          if (Array.isArray(obj[k])) {
            obj[k].forEach(function (v) {
              attr.addValue(v.toString());
            });
          } else {
            attr.addValue(obj[k].toString());
          }
          self.attributes.push(attr);
        });
      }
    };

    SearchEntry.prototype._json = function (j) {
      assert.ok(j);

      j.objectName = this.objectName.toString();
      j.attributes = [];
      this.attributes.forEach(function (a) {
        j.attributes.push(a.json || a);
      });

      return j;
    };

    SearchEntry.prototype._parse = function (ber) {
      assert.ok(ber);

      this.objectName = ber.readString();
      assert.ok(ber.readSequence());

      var end = ber.offset + ber.length;
      while (ber.offset < end) {
        var a = new Attribute();
        a.parse(ber);
        this.attributes.push(a);
      }

      return true;
    };

    SearchEntry.prototype._toBer = function (ber) {
      assert.ok(ber);

      ber.writeString(this.objectName.toString());
      ber.startSequence();
      this.attributes.forEach(function (a) {
        // This may or may not be an attribute
        ber = Attribute.toBer(a, ber);
      });
      ber.endSequence();

      return ber;
    };


    ///--- Exports

    module.exports = SearchEntry;

# request.js


    var assert = require('assert-plus');
    var util = require('util');

    var asn1 = require('asn1');

    var LDAPMessage = require('./message');
    var LDAPResult = require('./result');
    var dn = require('../dn');
    var filters = require('../filters');
    var Protocol = require('../protocol');


    ///--- Globals

    var Ber = asn1.Ber;


    ///--- API

    function SearchRequest(options) {
      options = options || {};
      assert.object(options);

      options.protocolOp = Protocol.LDAP_REQ_SEARCH;
      LDAPMessage.call(this, options);

      if (options.baseObject !== undefined) {
        this.baseObject = options.baseObject;
      } else {
        this.baseObject = dn.parse('');
      }
      this.scope = options.scope || 'base';
      this.derefAliases = options.derefAliases || Protocol.NEVER_DEREF_ALIASES;
      this.sizeLimit = options.sizeLimit || 0;
      this.timeLimit = options.timeLimit || 0;
      this.typesOnly = options.typesOnly || false;
      this.filter = options.filter || null;
      this.attributes = options.attributes ? options.attributes.slice(0) : [];
    }
    util.inherits(SearchRequest, LDAPMessage);
    Object.defineProperties(SearchRequest.prototype, {
      type: {
        get: function getType() { return 'SearchRequest'; },
        configurable: false
      },
      _dn: {
        get: function getDN() { return this.baseObject; },
        configurable: false
      },
      scope: {
        get: function getScope() {
          switch (this._scope) {
          case Protocol.SCOPE_BASE_OBJECT: return 'base';
          case Protocol.SCOPE_ONE_LEVEL: return 'one';
          case Protocol.SCOPE_SUBTREE: return 'sub';
          default:
            throw new Error(this._scope + ' is an invalid search scope');
          }
        },
        set: function setScope(val) {
          if (typeof (val) === 'string') {
            switch (val) {
            case 'base':
              this._scope = Protocol.SCOPE_BASE_OBJECT;
              break;
            case 'one':
              this._scope = Protocol.SCOPE_ONE_LEVEL;
              break;
            case 'sub':
              this._scope = Protocol.SCOPE_SUBTREE;
              break;
            default:
              throw new Error(val + ' is an invalid search scope');
            }
          } else {
            this._scope = val;
          }
        },
        configurable: false
      }
    });

    SearchRequest.prototype._parse = function (ber) {
      assert.ok(ber);

      this.baseObject = ber.readString();
      this.scope = ber.readEnumeration();
      this.derefAliases = ber.readEnumeration();
      this.sizeLimit = ber.readInt();
      this.timeLimit = ber.readInt();
      this.typesOnly = ber.readBoolean();

      this.filter = filters.parse(ber);

      // look for attributes
      if (ber.peek() === 0x30) {
        ber.readSequence();
        var end = ber.offset + ber.length;
        while (ber.offset < end)
          this.attributes.push(ber.readString().toLowerCase());
      }

      return true;
    };

    SearchRequest.prototype._toBer = function (ber) {
      assert.ok(ber);

      ber.writeString(this.baseObject.toString());
      ber.writeEnumeration(this._scope);
      ber.writeEnumeration(this.derefAliases);
      ber.writeInt(this.sizeLimit);
      ber.writeInt(this.timeLimit);
      ber.writeBoolean(this.typesOnly);

      var f = this.filter || new filters.PresenceFilter({attribute: 'objectclass'});
      ber = f.toBer(ber);

      ber.startSequence(Ber.Sequence | Ber.Constructor);
      if (this.attributes && this.attributes.length) {
        this.attributes.forEach(function (a) {
          ber.writeString(a);
        });
      }
      ber.endSequence();

      return ber;
    };

    SearchRequest.prototype._json = function (j) {
      assert.ok(j);

      j.baseObject = this.baseObject;
      j.scope = this.scope;
      j.derefAliases = this.derefAliases;
      j.sizeLimit = this.sizeLimit;
      j.timeLimit = this.timeLimit;
      j.typesOnly = this.typesOnly;
      j.filter = this.filter.toString();
      j.attributes = this.attributes;

      return j;
    };


    ///--- Exports

    module.exports = SearchRequest;

# response.js

    // Copyright 2011 Mark Cavage, Inc.  All rights reserved.

    var assert = require('assert-plus');
    var util = require('util');

    var LDAPResult = require('./result');
    var SearchEntry = require('./search_entry');
    var SearchReference = require('./search_reference');

    var dtrace = require('../dtrace');
    var parseDN = require('../dn').parse;
    var parseURL = require('../url').parse;
    var Protocol = require('../protocol');


    ///--- API

    function SearchResponse(options) {
      options = options || {};
      assert.object(options);

      options.protocolOp = Protocol.LDAP_REP_SEARCH;
      LDAPResult.call(this, options);

      this.attributes = options.attributes ? options.attributes.slice() : [];
      this.notAttributes = [];
      this.sentEntries = 0;
    }
    util.inherits(SearchResponse, LDAPResult);

    /**
     * Allows you to send a SearchEntry back to the client.
     *
     * @param {Object} entry an instance of SearchEntry.
     * @param {Boolean} nofiltering skip filtering notAttributes and '_' attributes.
     *                  Defaults to 'false'.
     */
    SearchResponse.prototype.send = function (entry, nofiltering) {
      if (!entry || typeof (entry) !== 'object')
        throw new TypeError('entry (SearchEntry) required');
      if (nofiltering === undefined)
        nofiltering = false;
      if (typeof (nofiltering) !== 'boolean')
        throw new TypeError('noFiltering must be a boolean');

      var self = this;

      if (entry instanceof SearchEntry || entry instanceof SearchReference) {
        if (!entry.messageID)
          entry.messageID = this.messageID;
        if (entry.messageID !== this.messageID)
          throw new Error('SearchEntry messageID mismatch');
      } else {
        if (!entry.attributes)
          throw new Error('entry.attributes required');

        var savedAttrs = {};
        var all = (self.attributes.indexOf('*') !== -1);
        Object.keys(entry.attributes).forEach(function (a) {
          var _a = a.toLowerCase();
          if (!nofiltering && _a.length && _a[0] === '_') {
            savedAttrs[a] = entry.attributes[a];
            delete entry.attributes[a];
          } else if (!nofiltering && self.notAttributes.indexOf(_a) !== -1) {
            savedAttrs[a] = entry.attributes[a];
            delete entry.attributes[a];
          } else if (all) {
            return;
          } else if (self.attributes.length && self.attributes.indexOf(_a) === -1) {
            savedAttrs[a] = entry.attributes[a];
            delete entry.attributes[a];
          }
        });

        var save = entry;
        entry = new SearchEntry({
          objectName: typeof (save.dn) === 'string' ? parseDN(save.dn) : save.dn,
          messageID: self.messageID,
          log: self.log
        });
        entry.fromObject(save);
      }

      try {
        if (this.log.debug())
          this.log.debug('%s: sending:  %j', this.connection.ldap.id, entry.json);

        this.connection.write(entry.toBer());
        this.sentEntries++;

        if (self._dtraceOp && self._dtraceId) {
          dtrace.fire('server-search-entry', function () {
            var c = self.connection || {ldap: {}};
            return [
              self._dtraceId || 0,
              (c.remoteAddress || ''),
              c.ldap.bindDN ? c.ldap.bindDN.toString() : '',
              (self.requestDN ? self.requestDN.toString() : ''),
              entry.objectName.toString(),
              entry.attributes.length
            ];
          });
        }

        // Restore attributes
        Object.keys(savedAttrs || {}).forEach(function (k) {
          save.attributes[k] = savedAttrs[k];
        });

      } catch (e) {
        this.log.warn(e, '%s failure to write message %j',
                      this.connection.ldap.id, this.json);
      }
    };

    SearchResponse.prototype.createSearchEntry = function (object) {
      assert.object(object);

      var entry = new SearchEntry({
        messageID: this.messageID,
        log: this.log,
        objectName: object.objectName || object.dn
      });
      entry.fromObject((object.attributes || object));
      return entry;
    };

    SearchResponse.prototype.createSearchReference = function (uris) {
      if (!uris)
        throw new TypeError('uris ([string]) required');

      if (!Array.isArray(uris))
        uris = [uris];

      for (var i = 0; i < uris.length; i++) {
        if (typeof (uris[i]) == 'string')
          uris[i] = parseURL(uris[i]);
      }

      var self = this;
      return new SearchReference({
        messageID: self.messageID,
        log: self.log,
        uris: uris
      });
    };


    ///--- Exports

    module.exports = SearchResponse;

# result.js

    // Copyright 2011 Mark Cavage, Inc.  All rights reserved.

    var assert = require('assert-plus');
    var util = require('util');

    var asn1 = require('asn1');

    var dtrace = require('../dtrace');
    var LDAPMessage = require('./message');
    var Protocol = require('../protocol');


    ///--- Globals

    var Ber = asn1.Ber;
    var BerWriter = asn1.BerWriter;


    ///--- API

    function LDAPResult(options) {
      options = options || {};
      assert.object(options);
      assert.optionalNumber(options.status);
      assert.optionalString(options.matchedDN);
      assert.optionalString(options.errorMessage);
      assert.optionalArrayOfString(options.referrals);

      LDAPMessage.call(this, options);

      this.status = options.status || 0; // LDAP SUCCESS
      this.matchedDN = options.matchedDN || '';
      this.errorMessage = options.errorMessage || '';
      this.referrals = options.referrals || [];

      this.connection = options.connection || null;
    }
    util.inherits(LDAPResult, LDAPMessage);
    Object.defineProperties(LDAPResult.prototype, {
      type: {
        get: function getType() { return 'LDAPResult'; },
        configurable: false
      }
    });

    LDAPResult.prototype.end = function (status) {
      assert.ok(this.connection);

      if (typeof (status) === 'number')
        this.status = status;

      var ber = this.toBer();
      if (this.log.debug())
        this.log.debug('%s: sending:  %j', this.connection.ldap.id, this.json);

      try {
        var self = this;
        this.connection.write(ber);

        if (self._dtraceOp && self._dtraceId) {
          dtrace.fire('server-' + self._dtraceOp + '-done', function () {
            var c = self.connection || {ldap: {}};
            return [
              self._dtraceId || 0,
              (c.remoteAddress || ''),
              c.ldap.bindDN ? c.ldap.bindDN.toString() : '',
              (self.requestDN ? self.requestDN.toString() : ''),
              status || self.status,
              self.errorMessage
            ];
          });
        }

      } catch (e) {
        this.log.warn(e, '%s failure to write message %j',
                      this.connection.ldap.id, this.json);
      }

    };

    LDAPResult.prototype._parse = function (ber) {
      assert.ok(ber);

      this.status = ber.readEnumeration();
      this.matchedDN = ber.readString();
      this.errorMessage = ber.readString();

      var t = ber.peek();

      if (t === Protocol.LDAP_REP_REFERRAL) {
        var end = ber.offset + ber.length;
        while (ber.offset < end)
          this.referrals.push(ber.readString());
      }

      return true;
    };

    LDAPResult.prototype._toBer = function (ber) {
      assert.ok(ber);

      ber.writeEnumeration(this.status);
      ber.writeString(this.matchedDN || '');
      ber.writeString(this.errorMessage || '');

      if (this.referrals.length) {
        ber.startSequence(Protocol.LDAP_REP_REFERRAL);
        ber.writeStringArray(this.referrals);
        ber.endSequence();
      }

      return ber;
    };

    LDAPResult.prototype._json = function (j) {
      assert.ok(j);

      j.status = this.status;
      j.matchedDN = this.matchedDN;
      j.errorMessage = this.errorMessage;
      j.referrals = this.referrals;

      return j;
    };


    ///--- Exports

    module.exports = LDAPResult;

# parser.js

    ///--- Globals

    var Ber = asn1.Ber;
    var BerReader = asn1.BerReader;


    ///--- API

    function Parser(options) {
      assert.object(options);
      assert.object(options.log);

      EventEmitter.call(this);

      this.buffer = null;
      this.log = options.log;
    }
    util.inherits(Parser, EventEmitter);

    Parser.prototype.write = function (data) {
      if (!data || !Buffer.isBuffer(data))
        throw new TypeError('data (buffer) required');

      var nextMessage = null;
      var self = this;

      function end() {
        if (nextMessage)
          return self.write(nextMessage);

        return true;
      }

      self.buffer = (self.buffer ? Buffer.concat([self.buffer, data]) : data);

      var ber = new BerReader(self.buffer);

      var foundSeq = false;
      try {
        foundSeq = ber.readSequence();
      } catch (e) {
        this.emit('error', e);
      }

      if (!foundSeq || ber.remain < ber.length) {
        // ENOTENOUGH
        return false;
      } else if (ber.remain > ber.length) {
        // ETOOMUCH
        // This is sort of ugly, but allows us to make miminal copies
        nextMessage = self.buffer.slice(ber.offset + ber.length);
        ber._size = ber.offset + ber.length;
        assert.equal(ber.remain, ber.length);
      }

      // If we're here, ber holds the message, and nextMessage is temporarily
      // pointing at the next sequence of data (if it exists)
      self.buffer = null;

      var message;
      try {
        // Bail here if peer isn't speaking protocol at all
        message = this.getMessage(ber);

        if (!message) {
          return end();
        }
        message.parse(ber);
      } catch (e) {
        this.emit('error', e, message);
        return false;
      }

      this.emit('message', message);
      return end();
    };

    Parser.prototype.getMessage = function (ber) {
      assert.ok(ber);

      var self = this;

      var messageID = ber.readInt();
      var type = ber.readSequence();

      var Message;
      switch (type) {

      case Protocol.LDAP_REQ_ABANDON:
        Message = AbandonRequest;
        break;

      case Protocol.LDAP_REQ_ADD:
        Message = AddRequest;
        break;

      case Protocol.LDAP_REP_ADD:
        Message = AddResponse;
        break;

      case Protocol.LDAP_REQ_BIND:
        Message = BindRequest;
        break;

      case Protocol.LDAP_REP_BIND:
        Message = BindResponse;
        break;

      case Protocol.LDAP_REQ_COMPARE:
        Message = CompareRequest;
        break;

      case Protocol.LDAP_REP_COMPARE:
        Message = CompareResponse;
        break;

      case Protocol.LDAP_REQ_DELETE:
        Message = DeleteRequest;
        break;

      case Protocol.LDAP_REP_DELETE:
        Message = DeleteResponse;
        break;

      case Protocol.LDAP_REQ_EXTENSION:
        Message = ExtendedRequest;
        break;

      case Protocol.LDAP_REP_EXTENSION:
        Message = ExtendedResponse;
        break;

      case Protocol.LDAP_REQ_MODIFY:
        Message = ModifyRequest;
        break;

      case Protocol.LDAP_REP_MODIFY:
        Message = ModifyResponse;
        break;

      case Protocol.LDAP_REQ_MODRDN:
        Message = ModifyDNRequest;
        break;

      case Protocol.LDAP_REP_MODRDN:
        Message = ModifyDNResponse;
        break;

      case Protocol.LDAP_REQ_SEARCH:
        Message = SearchRequest;
        break;

      case Protocol.LDAP_REP_SEARCH_ENTRY:
        Message = SearchEntry;
        break;

      case Protocol.LDAP_REP_SEARCH_REF:
        Message = SearchReference;
        break;

      case Protocol.LDAP_REP_SEARCH:
        Message = SearchResponse;
        break;

      case Protocol.LDAP_REQ_UNBIND:
        Message = UnbindRequest;
        break;

      default:
        this.emit('error',
                  new Error('Op 0x' + (type ? type.toString(16) : '??') +
                            ' not supported'),
                  new LDAPResult({
                    messageID: messageID,
                    protocolOp: type || Protocol.LDAP_REP_EXTENSION
                  }));

        return false;
      }


      return new Message({
        messageID: messageID,
        log: self.log
      });
    };


    ///--- Exports

    module.exports = Parser;





