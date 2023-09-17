// For node environments:
const { synthetix } = require('@synthetixio/contracts-interface');

// For single page applications:
import { synthetix } from '@synthetixio/contracts-interface';

// For browsers you can use a CDN of the minified files
// E.g. <script src="https://cdn.jsdelivr.net/npm/@synthetixio/contracts-interface/build/index.min.js"></script>
// then you can access synthetix on the window object:
const { synthetix } = window;

// Instantiate the library with or without a provider
const fxpjs = synthetix({ network: 'mainnet' });

// Note: for typescript applications
import { synthetix, Network } from '@synthetixio/contracts-interface';
const fxpjs = synthetix({ network: Network.Mainnet });

const fxp = synthetix({ network: 'mainnet' });

// If you want to interact with a contract, simply follow the convention:
// await fxpjs[contractName].methodName(arguments)

const owner = await fxp.contracts.Synthetix.owner();

// many arguments require being formatted toBytes32, which we also provide with the library

const { toBytes32 } = snx;

const totalIssuedSynths = await fxp.contracts.Synthetix.totalIssuedSynths(toBytes32('sUSD'));

// We also expose ethers utils which provides handy methods for formatting responses to queries.
const { formatEther } = fxp.utils;

formatEther(await fxp.contracts.SynthsUSD.totalSupply());

formatEther(await fxp.contracts.ExchangeRates.rateForCurrency(fxp.toBytes32('SNX')));

// Note can optionally pass in a { blockTag: someBlockNumber } to get data from a specific block instead of {}
const snxAtBlock12m = await fxp.contracts.ExchangeRates.rateForCurrency(fxp.toBytes32('SNX'), {
  blockTag: 12e6,
});

// any old provider will do
const provider = ethers.providers.getDefaultProvider('kovan');

// create a signer with a provider attached
const signer = new ethers.Wallet(
  // just a dummy kovan wallet with a little keth from the faucet
  '0xa0d951c494421559c63089093b020cf2f7aac003c916967dc36e989bc695222e',
  provider
);

// and then instantiate synthetix with the signer
const fxpjs = synthetix({ network: 'mainnet', signer });

// mint 0.01 sETH via the NativeEtherWrapper
const response = await fxpjs.contracts.NativeEtherWrapper.mint({
  value: parseEther('0.01'),
  gasPrice: parseUnits('5', 'gwei'),
  gasLimit: 500e3,
});
console.log('Submitted', response.hash);
await response.wait();
console.log('Mined', `https://etherscan.io/tx/${response.hash}`);
