```
import React, {Component} from 'react';

import {StyleSheet, Dimensions, View, Text} from 'react-native';
import {WebView} from 'react-native-webview';

class App extends Component {

    constructor(props) {
        super(props);
        this.state = {
            showLogin: true,
            token: "",
            publicKey: 'YOUR_PUBLIC_KEY',
            redirectUrl: 'YOUR_REDIRECT_URL',
            domain: 'YOUR_DOMAIN',
        }
    }

    parseUrl(url) {
        var regex = /[?&]([^=#]+)=([^&#]*)/g,
            params = {},
            match;
        while (match = regex.exec(url)) {
            params[match[1]] = match[2];
        }

        return params;
    }

    render() {
        return (
            <View style={styles.container}>
                {this.state.showLogin &&
                <WebView
                    style={{width: Dimensions.get('window').width - 15, marginTop: 35}}
                    javaScriptEnabled={true}
                    scrollEnabled={false}
                    automaticallyAdjustContentInsets={true}
                    source={{uri: 'https://login.zotlo.com/?publicKey=' + this.state.publicKey + '&redirectUri=' + this.state.redirectUrl + '&domain=' + this.state.domain}}
                    onShouldStartLoadWithRequest={state => {
                        var params = this.parseUrl(state.url);
                        if ('token' in params && params.token != '') {
                            console.log("Single use token: " + params.token);
                            this.setState({showLogin: false})
                            this.setState({token: params.token})
                            return false;
                        } else {
                            return true;
                        }
                    }}

                />
                }

                {this.state.showLogin == false &&
                <Text>Token : {this.state.token}</Text>
                }
            </View>
        );
    }
}

const styles = StyleSheet.create({
container: {
flex: 1,
backgroundColor: '#fff',
alignItems: 'center',
justifyContent: 'center',
},
});


export default App
```

