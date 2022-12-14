- User creates NFT
- User inputs an email address to send the NFT to
  - !! Throttle this endpoint. Significant spam risk
  - Take email and use it as the base of a derived address
    - Store this email (or email hash) to public key somewhere on the backend
  - Send an email to the recipient containing a code to claim the NFT
- User opens email and clicks "Claim NFT" link containing the code
  - Website makes a claim call to the backend with the code
  - Code resolves to the public key of the code (requires the code <> email link to be perfectly tight)
  - If the derived account does not exist first create it and signer swap out the master signer with a custodial service signer
    - This may require some thought either here or in the next step as to what the signer setup actually is
  - If or once the account exists create a session signer for the user along with a future transaction which will allow the service to take back ownership after the session ends (likely 24 hrs)
  - (signer secret should be generated client side and added server side so the server doesn't know the secret but can remove it after the session is over)
    - (there may be something here with the code sent along in the email where some code isn't stored on the server but is sent along to the client via email)
    - (essentially we just need a sufficiently secure method for allowing the user to take over ownership of an email based account for short(ish) sessions)
    - (bonus points if the server never has full custody of accounts at rest. Doubtful this is possible + user friendly but it's worth considering)
      - (I think it would require the user to maintain some secret-to-the-server secret between sessions)
      - (It's possible we could maintain full custody solution behind a {x}hr wait period alongside an instant shared custody in the case the user remembers their secret-to-the-server secret. aka a kind of cryptographically delayed account reset)
        - (this should likely be initiated, warned and secured via the email address itself)
        - (actually kinda interesting and may work pretty well except in cases where there is no access to the email address itself)
          - (it's less important that a client has a secret and more important that they have private abilities i.e. sending emails from a known address)
            - (private actions vs private info)
  - User then receives an XDR to claim their NFT on the client-side