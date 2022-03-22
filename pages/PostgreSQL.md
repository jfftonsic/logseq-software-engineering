tags:: Database, Relational Databases

- [[Common Table Expression]]
- types:
	- <table border="1">
	      <colgroup>
	          <col>
	          <col>
	          <col>
	      </colgroup>
	      <thead>
	          <tr>
	              <th>Name</th>
	              <th>Aliases</th>
	              <th>Description</th>
	          </tr>
	      </thead>
	      <tbody>
	          <tr>
	              <td><tt>bigint</tt></td>
	              <td><tt>int8</tt></td>
	              <td>signed eight-byte integer</td>
	          </tr>
	          <tr>
	              <td><tt>bigserial</tt></td>
	              <td><tt>serial8</tt></td>
	              <td>autoincrementing eight-byte integer</td>
	          </tr>
	          <tr>
	              <td><tt>bit [ (<tt>n</tt>) ]</tt>
	              </td>
	              <td>&nbsp;</td>
	              <td>fixed-length bit string</td>
	          </tr>
	          <tr>
	              <td><tt>bit varying [ (<tt>n</tt>) ]</tt>
	              </td>
	              <td><tt>varbit [ (<tt>n</tt>) ]</tt>
	              </td>
	              <td>variable-length bit string</td>
	          </tr>
	          <tr>
	              <td><tt>boolean</tt></td>
	              <td><tt>bool</tt></td>
	              <td>logical Boolean (true/false)</td>
	          </tr>
	          <tr>
	              <td><tt>box</tt></td>
	              <td>&nbsp;</td>
	              <td>rectangular box on a plane</td>
	          </tr>
	          <tr>
	              <td><tt>bytea</tt></td>
	              <td>&nbsp;</td>
	              <td>binary data (<span>"byte array"</span>)</td>
	          </tr>
	          <tr>
	              <td><tt>character [ (<tt>n</tt>) ]</tt>
	              </td>
	              <td><tt>char [ (<tt>n</tt>) ]</tt>
	              </td>
	              <td>fixed-length character string</td>
	          </tr>
	          <tr>
	              <td><tt>character varying [ (<tt>n</tt>) ]</tt>
	              </td>
	              <td><tt>varchar [ (<tt>n</tt>) ]</tt>
	              </td>
	              <td>variable-length character string</td>
	          </tr>
	          <tr>
	              <td><tt>cidr</tt></td>
	              <td>&nbsp;</td>
	              <td>IPv4 or IPv6 network address</td>
	          </tr>
	          <tr>
	              <td><tt>circle</tt></td>
	              <td>&nbsp;</td>
	              <td>circle on a plane</td>
	          </tr>
	          <tr>
	              <td><tt>date</tt></td>
	              <td>&nbsp;</td>
	              <td>calendar date (year, month, day)</td>
	          </tr>
	          <tr>
	              <td><tt>double precision</tt></td>
	              <td><tt>float8</tt></td>
	              <td>double precision floating-point number (8 bytes)</td>
	          </tr>
	          <tr>
	              <td><tt>inet</tt></td>
	              <td>&nbsp;</td>
	              <td>IPv4 or IPv6 host address</td>
	          </tr>
	          <tr>
	              <td><tt>integer</tt></td>
	              <td><tt>int</tt>,<span>&nbsp;</span><tt>int4</tt></td>
	              <td>signed four-byte integer</td>
	          </tr>
	          <tr>
	              <td><tt>interval [<span>&nbsp;</span><tt>fields</tt><span>&nbsp;</span>] [ (<tt>p</tt>) ]</tt>
	              </td>
	              <td>&nbsp;</td>
	              <td>time span</td>
	          </tr>
	          <tr>
	              <td><tt>json</tt></td>
	              <td>&nbsp;</td>
	              <td>textual JSON data</td>
	          </tr>
	          <tr>
	              <td><tt>jsonb</tt></td>
	              <td>&nbsp;</td>
	              <td>binary JSON data, decomposed</td>
	          </tr>
	          <tr>
	              <td><tt>line</tt></td>
	              <td>&nbsp;</td>
	              <td>infinite line on a plane</td>
	          </tr>
	          <tr>
	              <td><tt>lseg</tt></td>
	              <td>&nbsp;</td>
	              <td>line segment on a plane</td>
	          </tr>
	          <tr>
	              <td><tt>macaddr</tt></td>
	              <td>&nbsp;</td>
	              <td>MAC (Media Access Control) address</td>
	          </tr>
	          <tr>
	              <td><tt>money</tt></td>
	              <td>&nbsp;</td>
	              <td>currency amount</td>
	          </tr>
	          <tr>
	              <td><tt>numeric [ (<tt>p</tt>,<span>&nbsp;</span><tt>s</tt>) ]</tt>
	              </td>
	              <td><tt>decimal [ (<tt>p</tt>,<span>&nbsp;</span><tt>s</tt>) ]</tt>
	              </td>
	              <td>exact numeric of selectable precision</td>
	          </tr>
	          <tr>
	              <td><tt>path</tt></td>
	              <td>&nbsp;</td>
	              <td>geometric path on a plane</td>
	          </tr>
	          <tr>
	              <td><tt>pg_lsn</tt></td>
	              <td>&nbsp;</td>
	              <td><span>PostgreSQL</span><span>&nbsp;</span>Log Sequence Number</td>
	          </tr>
	          <tr>
	              <td><tt>point</tt></td>
	              <td>&nbsp;</td>
	              <td>geometric point on a plane</td>
	          </tr>
	          <tr>
	              <td><tt>polygon</tt></td>
	              <td>&nbsp;</td>
	              <td>closed geometric path on a plane</td>
	          </tr>
	          <tr>
	              <td><tt>real</tt></td>
	              <td><tt>float4</tt></td>
	              <td>single precision floating-point number (4 bytes)</td>
	          </tr>
	          <tr>
	              <td><tt>smallint</tt></td>
	              <td><tt>int2</tt></td>
	              <td>signed two-byte integer</td>
	          </tr>
	          <tr>
	              <td><tt>smallserial</tt></td>
	              <td><tt>serial2</tt></td>
	              <td>autoincrementing two-byte integer</td>
	          </tr>
	          <tr>
	              <td><tt>serial</tt></td>
	              <td><tt>serial4</tt></td>
	              <td>autoincrementing four-byte integer</td>
	          </tr>
	          <tr>
	              <td><tt>text</tt></td>
	              <td>&nbsp;</td>
	              <td>variable-length character string</td>
	          </tr>
	          <tr>
	              <td><tt>time [ (<tt>p</tt>) ] [ without time zone ]</tt>
	              </td>
	              <td>&nbsp;</td>
	              <td>time of day (no time zone)</td>
	          </tr>
	          <tr>
	              <td><tt>time [ (<tt>p</tt>) ] with time zone</tt>
	              </td>
	              <td><tt>timetz</tt></td>
	              <td>time of day, including time zone</td>
	          </tr>
	          <tr>
	              <td><tt>timestamp [ (<tt>p</tt>) ] [ without time zone ]</tt>
	              </td>
	              <td>&nbsp;</td>
	              <td>date and time (no time zone)</td>
	          </tr>
	          <tr>
	              <td><tt>timestamp [ (<tt>p</tt>) ] with time zone</tt>
	              </td>
	              <td><tt>timestamptz</tt></td>
	              <td>date and time, including time zone</td>
	          </tr>
	          <tr>
	              <td><tt>tsquery</tt></td>
	              <td>&nbsp;</td>
	              <td>text search query</td>
	          </tr>
	          <tr>
	              <td><tt>tsvector</tt></td>
	              <td>&nbsp;</td>
	              <td>text search document</td>
	          </tr>
	          <tr>
	              <td><tt>txid_snapshot</tt></td>
	              <td>&nbsp;</td>
	              <td>user-level transaction ID snapshot</td>
	          </tr>
	          <tr>
	              <td><tt>uuid</tt></td>
	              <td>&nbsp;</td>
	              <td>universally unique identifier</td>
	          </tr>
	          <tr>
	              <td><tt>xml</tt></td>
	              <td>&nbsp;</td>
	              <td>XML data</td>
	          </tr>
	      </tbody>
	  </table>
-
-