# Annexes

## Elegant

### Style

```elm
{-| Contains all style for an element used with Elegant.
-}
type Style
    = Style
        { position : Maybe Position
        , left : Maybe SizeUnit
        , top : Maybe SizeUnit
        , bottom : Maybe SizeUnit
        , right : Maybe SizeUnit
        , textColor : Maybe Color
        , backgroundColor : Maybe Color
        , backgroundImages : List BackgroundImage
        , borderBottomColor : Maybe Color
        , borderBottomWidth : Maybe SizeUnit
        , borderBottomStyle : Maybe Border
        , borderLeftColor : Maybe Color
        , borderLeftWidth : Maybe SizeUnit
        , borderLeftStyle : Maybe Border
        , borderTopColor : Maybe Color
        , borderTopWidth : Maybe SizeUnit
        , borderTopStyle : Maybe Border
        , borderRightColor : Maybe Color
        , borderRightWidth : Maybe SizeUnit
        , borderRightStyle : Maybe Border
        , borderBottomLeftRadius : Maybe SizeUnit
        , borderBottomRightRadius : Maybe SizeUnit
        , borderTopLeftRadius : Maybe SizeUnit
        , borderTopRightRadius : Maybe SizeUnit
        , outlineColor : Maybe Color
        , outlineStyle : Maybe Border
        , outlineWidth : Maybe SizeUnit
        , boxShadow : Maybe BoxShadow
        , paddingRight : Maybe SizeUnit
        , paddingLeft : Maybe SizeUnit
        , paddingBottom : Maybe SizeUnit
        , paddingTop : Maybe SizeUnit
        , marginRight : Maybe (Either SizeUnit Auto)
        , marginLeft : Maybe (Either SizeUnit Auto)
        , marginBottom : Maybe (Either SizeUnit Auto)
        , marginTop : Maybe (Either SizeUnit Auto)
        , display : Maybe Display
        , flexGrow : Maybe Int
        , flexShrink : Maybe Int
        , flexBasis : Maybe (Either SizeUnit Auto)
        , flexWrap : Maybe FlexWrap
        , flexDirection : Maybe FlexDirection
        , opacity : Maybe Float
        , overflowX : Maybe Overflow
        , overflowY : Maybe Overflow
        , textOverflow : Maybe TextOverflow
        , listStyleType : Maybe ListStyleType
        , verticalAlign : Maybe String
        , textAlign : Maybe TextAlign
        , textTransform : Maybe TextTransform
        , textDecoration : Maybe TextDecoration
        , whiteSpace : Maybe WhiteSpace
        , lineHeight : Maybe (Either SizeUnit Normal)
        , fontWeight : Maybe Int
        , fontStyle : Maybe FontStyle
        , fontSize : Maybe SizeUnit
        , fontFamily : Maybe FontFamily
        , alignItems : Maybe AlignItems
        , alignSelf : Maybe AlignSelf
        , justifyContent : Maybe JustifyContent
        , width : Maybe SizeUnit
        , maxWidth : Maybe SizeUnit
        , minWidth : Maybe SizeUnit
        , height : Maybe SizeUnit
        , maxHeight : Maybe SizeUnit
        , minHeight : Maybe SizeUnit
        , zIndex : Maybe Int
        , cursor : Maybe String
        , visibility : Maybe Visibility
        , boxSizing : Maybe String
        , screenWidths : List ScreenWidth
        , userSelect : Maybe UserSelect
        }
```



