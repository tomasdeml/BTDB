# Known Issues
## Dictionaries
### Keys
- do not use string, sbyte, byte[] in compound keys
- do not use `DateTime` instances with other than `DateTime.Utc` date time kind in compound keys - it is checked on all places except compound keys by BTDB automatically

TODO

TODO Convert to Wiki page in the end?
