class ValorantAgentInstalocker:
 def __init__(self):
 global cfg_path
 with open(cfg_path) as json_file:
 data = json.load(json_file)

 try:
 self.region = data['instantlocker']["region"].lower()
 self.preferred_agent =  data['instantlocker']["preferred_agent"].lower()
        except:
            exiting()
        self.agents = {
            "jett": "add6443a-41bd-e414-f6ad-e58d267f4e95",
            "reyna": "a3bfb853-43b2-7238-a4f1-ad90e9e46bcc",
            "raze": "f94c3b30-42be-e959-889c-5aa313dba261",
            "yoru": "7f94d92c-4234-0a36-9646-3a87eb8b5c89",
            "phoenix": "eb93336a-449b-9c1b-0a54-a891f7921d69",
            "neon": "bb2a4828-46eb-8cd1-e765-15848195d751",
            "breach": "5f8d3a7f-467b-97f3-062c-13acf203c006",
            "skye": "6f2a04ca-43e0-be17-7f36-b3908627744d",
            "sova": "320b2a48-4d9b-a075-30f1-1f93a9b638fa",
            "kayo": "601dbbe7-43ce-be57-2a40-4abd24953621",
            "killjoy": "1e58de9c-4950-5125-93e9-a0aee9f98746",
            "cypher": "117ed9e3-49f3-6512-3ccf-0cada7e3823b",
            "sage": "569fdd95-4d10-43ab-ca70-79becc718b46",
            "chamber": "22697a3d-45bf-8dd7-4fec-84a9e28c69d7",
            "omen": "8e253930-4c05-31dd-1b6c-968525494517",
            "brimstone": "9f0d8ba9-4140-b941-57d3-a7ad57c6b417",
            "astra": "41fb69c1-4189-7b37-f117-bcaf1e96f1bf",
            "viper": "707eab51-4836-f488-046a-cda6bf494859",
            "fade": "dade69b4-4f5a-8528-247b-219e5a1facd6",
            "gekko": "e370fa57-4757-3604-3648-499e1f642d3f",
            "harbor": "95b78ed7-4637-86d9-7e41-71ba8c293152",
            "deadlock": "cc8b64c8-4b25-4ff9-6e7f-37b4da43d235"
        }
 
        self.seenMatches = []
 
    def initialize_client(self):
        while True:
            try:
                self.client = Client(region=self.region)
                self.client.activate()
            except:
                print("open game dummy")
            else:
                self.run_instalocker()
            time.sleep(2)
 
 
    def choose_preferred_agent(self):
        while True:
            try:
                if self.preferred_agent in self.agents.keys():
                    return self.preferred_agent
                else:
                    print("Invalid Agent")
            except:
                print("Input Error")
 
    def run_instalocker(self):
        print("Waiting for Agent Select")
        while True:
            time.sleep(1)
            try:
                session_state = self.client.fetch_presence(self.client.puuid)['sessionLoopState']
                match_id = self.client.pregame_fetch_match()['ID']
 
                if session_state == "PREGAME" and match_id not in self.seenMatches:
                    print('Agent Select Found')
                    preferred_agent = self.choose_preferred_agent()
                    agent_id = self.agents[preferred_agent]
                    self.client.pregame_select_character(agent_id)
                    self.client.pregame_lock_character(agent_id)
                    self.seenMatches.append(match_id)
                    print(f'Successfully Locked {preferred_agent.capitalize()}')
            except Exception as e:
                print('', end='')  # goofy
