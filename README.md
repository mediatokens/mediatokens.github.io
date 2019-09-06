<script src="https://code.jquery.com/jquery-2.1.1.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/uikit/3.1.4/js/uikit.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/uikit/3.1.4/js/uikit-icons.min.js"></script>
<script src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/604284/web3.js"></script>

<div class="uk-section>
<h1>
Peperium
</h1>
</div>

<div class="uk-section>
<h1>
Archetype

<div id="arxMemes" class=" uk-child-width-1-3 uk-width-5-6 uk-text-center uk-margin-large-left uk-margin-large-right" uk-grid>

  </div>
</h1>


</div>


<script>
var State = {
   memes:{value:new Array(),watchers:[]},
   orders:{value:[],watchers:[]},
   completed_trades:{value:[],watchers:[]}
};

function UpdateState(key,value) {
   if(State[key].value === value)
    return;
  if (!((State[key].value) instanceof Array)) {
      console.log('Not array')
      State[key].value = value;
  } else {
      console.log('Array')
      State[key].value.push(value);
  }
  for (w in State[key].watchers) {
    State[key].watchers[w](value)
  }
}

function On(key,watcher) {
  State[key].watchers.push(watcher);
}

$().ready(function() {
  window.web3 = new Web3(
      new Web3.providers.WebsocketProvider("wss://mainnet.infura.io/v3/87c7cd2d74994258b70871d1c72a48dd")
    );

    makeArx();
    loadListings();
});
 var arxAddress = '0x65B14D471F85FB3DfDD77865b0323E59E7C18385';
 var pprAddress = '';

 var memeABI = [
  {
    "constant": false,
    "inputs": [
      {
        "name": "_initialAmount",
        "type": "uint256"
      },
      {
        "name": "_name",
        "type": "string"
      },
      {
        "name": "_symbol",
        "type": "string"
      },
      {
        "name": "_desc",
        "type": "string"
      },
      {
        "name": "_ipfshash",
        "type": "string"
      },
      {
        "name": "_type",
        "type": "uint256"
      },
      {
        "name": "_recv",
        "type": "address"
      }
    ],
    "name": "createMeme",
    "outputs": [
      {
        "name": "",
        "type": "address"
      }
    ],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [],
    "name": "MemeCount",
    "outputs": [
      {
        "name": "",
        "type": "uint256"
      }
    ],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [
      {
        "name": "",
        "type": "uint256"
      }
    ],
    "name": "Memes",
    "outputs": [
      {
        "name": "",
        "type": "address"
      }
    ],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [],
    "name": "owner",
    "outputs": [
      {
        "name": "",
        "type": "address"
      }
    ],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": false,
    "inputs": [
      {
        "name": "newOwner",
        "type": "address"
      }
    ],
    "name": "transferOwnership",
    "outputs": [],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "function"
  },
  {
    "inputs": [],
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "constructor"
  },
  {
    "payable": false,
    "stateMutability": "nonpayable",
    "type": "fallback"
  },
  {
    "anonymous": false,
    "inputs": [
      {
        "indexed": false,
        "name": "token",
        "type": "address"
      },
      {
        "indexed": false,
        "name": "name",
        "type": "string"
      },
      {
        "indexed": false,
        "name": "issuance",
        "type": "uint256"
      }
    ],
    "name": "MemeCreated",
    "type": "event"
  }
];

function loadListings() {


  arxInstance.methods.MemeCount().call().then(function (count) {
    var loop = count - 1;

    while(loop >= 0) {
      factory.methods.projects(loop).call().then(function (address) {
        UpdateState('memes',address);
      })
      loop--;
    }

  })

}


 function makePeperium() {


 }

 function makeArx() {
   window.arxInstance = new web3.eth.Contract(memeABI,arxAddress);
 }

 On('memes', function (v){
  console.log('Meme loaded: ' + v);
   $("#arxMemes").append('<div class=""><div class="uk-card uk-card-small uk-card-primary bluebg"><div class="uk-card-body"><img src="'+ '' +'"></img></div><div class="uk-card-footer">'+ v +'</div></div> </div>'

} );

</script>
