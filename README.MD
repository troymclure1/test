 beforeAll(() => {
    jest.mock('jwks-rsa', () => {
      return jest.fn().mockImplementationOnce(() => {
        return {
          getSigningKey: (_, callback) => {
            callback(null, {
              publicKey: 'public-key-fake',
            });
          },
        };
      });
    });
  });
  
    const parsedCert = await new Promise((resolve) => {
      client.getSigningKey(kid, (err, key) => {
        //console.log('the kid is:: ', kid);
        console.log('the key is:: ', key);
        const cert = key.publicKey
          .match(/.{1,64}/g)
          .map((line) => `${line}`)
          .join('\n');

        // stringify the output
        const parseCert = JSON.stringify(cert);

        resolve(parseCert);
      });
    });
    
    
