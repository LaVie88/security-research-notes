# Authorization Code Expiration Gap

**Library:** Authlib
**RFC:** 6749 Section 4.1.2
**CVSS:** 7.5 (High)
**Status:** Private Disclosure

---

## Problem

Authlib does not enforce authorization code expiration at framework level.

RFC 6749 says:
> "The authorization code MUST expire shortly after it is issued"

## What happens

If developer doesn't implement `expires_at`, authorization code stays valid forever.

This violates:
- RFC 6749 Section 4.1.2 (MUST expire)
- CWE-613 (Insufficient Session Expiration)

## Comparison

| Library | Default Expiration |
|---------|-------------------|
| Authlib | None |
| Django OAuth Toolkit | 600 seconds |
| Spring Security | 60 seconds |
| Node oauth2-server | 300 seconds |

## Proposed Fix

Framework should enforce 10-minute default expiration:

```python
class AuthorizationCodeGrant(BaseGrant):
    AUTHORIZATION_CODE_EXPIRES_IN = 600  # 10 minutes
