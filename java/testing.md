#### Junit testy
stačí bejt v classe a pak dát generate>test a zakliknout metody
jedná se pouze o "mockování" takže se nevolaj ty requesty doopravdy

používá se anotace
###### @Mock
vytvoří "fake" instanci třídy

```
@Mock
private AddressRepository addressRepository;
```
###### @InjectMocks
používá se na injectnutí všech dependencies do classy kterou testuju aniž bych musel bejt připojenej k db apod

```
@Mock
private AddressRepository addressRepository;

@InjectMocks
private AddressService addressService;
```

###### @Test
pro každou metodu
většinou se to jmenuje podle metody kterou testuju nebo co by měla testovat
jsou void protože nic nevrací

```
@Test
    void createAddress() {
        Address address = new Address();
        address.setId(1);
        address.setCity("Plzeň");
        address.setStreet("Moravská");
        address.setStreetNumber(154);

        when(addressRepository.save(address)).thenReturn(address);

        Address result = addressService.createAddress(address);

        assertNotNull(result);
        assertEquals(address, result);
        verify(addressRepository, times(1)).save(address);
    }
```
- tenhle test "vytváří adresu"
- nejdřív si vytvořím objekt
- pak použiju when kterej pokud zavolám něco tak to vráti ty "fake" data
- takže tady to funguje tak, že pokud volám save z JPA což je databáze related funkce tak to jakoby vrátí ten objekt jako kdybych ho tam fakt přidal
- a pak to jen uložím do result a používám metody na kontrolu
- assertNotNull je asi jasnej
- asserEquals taky
- verify funguje v tomhle případě tak, že kontroluje kolikrát se volala metoda save, pokud to není 1 tak to znamená že test neprošel

 #### Integrační testy
 - na rozdíl od Junit testů tyhle testujou api jako takový
 - pokud tam je autorizace tak se musí získat login/token záleží
 - jelikož je tam test api jako takovýho tak to samozřejmě trvá dýl to otestovat protože se to všechno musí pustit atd

 ```
//abych mohl otestovat nějakou metodu tak musím nejdřív získat jwt token takže
//tímhle si to usnadním a nebudu si muset získávat ten token pro každej endpoint kterej testuju
    @BeforeAll
    void setUp() throws Exception {
        token = obtainAccessToken("username", "password");
    }
```
Anotace
- @AutoConfigureMockMvc
- @SpringBootTest
- @ExtendWith(SpringExtension.class)

##### Ukázkový test
```
    @Test
    void getAllAddresses() throws Exception {

        mockMvc.perform(MockMvcRequestBuilders.get("/api/address")
                        .header("Authorization", "Bearer " + token))
                .andDo(MockMvcResultHandlers.print())
                .andExpect(MockMvcResultMatchers.status().isOk());
    }
```
tady se používá MockMvc na zavolání endpointu, do headeru se pak dává token kterej se generuje před každým testem jak lze vidět nahoře <br />
###### Metody
- andDo() <br />
bere ResultHandler a vykoná cokoliv mu requestne, tady třeba print nebo log
- andExpect() <br />
bere ResultMatcher a porovná to co vrátil request s tím co se očekává 
- andReturn() <br />
vrací MvcResult jako objekt kterej obsahuje data, který vrátil ten request

#### Contract Testy
jedná se o typ testů které ověřují zda služby dodržují "kontrakt", ten definuje jak by měla vypadat komunikace mezi službami nebo systémy, například formát requestů a responsí, HTTP metody a headery nebo stavové kódy (200,400,...)

![image](https://github.com/user-attachments/assets/96429509-a771-4067-ab50-3704cf10fb26)

nemusí to vždy být BE a FE tohle je jen příklad

používají se v mikroslužbových architekturách, kde je důležité zajistit, že změny v jedné službě nezpůsobí problémy v jiných službách.

jak vypadá contract:
- interface: sada metod, funkcí nebo api endpointů, které jsou exposenuty servisou. Obsahuje jména, parametry a co to vrací
- data formáty: definuje strukturu nebo typy posílaných dat, např JSON
- předpoklady: podmínky, které musí být splněny než se cokoliv začne volat (autentikace apod)
- postpodmínky: popsání očekávaného stavu po zavolání metod nebo funkcí a očekávané response
- invarianty: pevně stanovená pravidla, která se nemění
- handlování chyb

Používají se protože umožňují developerům pracovat na jedné službě aniž by museli znát detaily implementace jiných služeb. Navíc zajišťují, že služby spolu mohou komunikovat i když jsou vyvíjeny nezávisle.

Na Contract testy se například používá Pact, Spring Cloud Contract nebo OpenAPI.
