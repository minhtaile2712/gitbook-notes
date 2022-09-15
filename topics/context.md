# Context

Context (ThemeContext, AuthContext, = createContext())

........................ Reducer = (state, action) => { switch (action.type) { case 'ACTION1': return newState; default: return state; }

Store = createStore(Reducer);

App: Store.subscribe; Store.getState Store.dispatch({type: 'ACTION1'}) ..............................

Provider (ThemeProvider, AuthProvider, = ThemeContext.Provider,...)
