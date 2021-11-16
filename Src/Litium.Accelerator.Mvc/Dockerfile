#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM registry.litium.cloud/runtime/litium:net5-latest AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443
ENV Litium__License="eyJhbGciOiJSUzI1NiIsImtpZCI6IkI3N0JFNEM5N0IzMDNGREJBRTNBRjY4NjEzN0NBMjU1QjhGQjNDQkMiLCJ4NXQiOiJ0M3ZreVhzd1A5dXVPdmFHRTN5aVZiajdQTHciLCJ0eXAiOiJKV1QifQ.eyJpZCI6ImFiMTI5OGE5LWYzNmEtNDIxOC04MzYyLWUxOWYwZjM2NWM2NiIsInZlcnNpb24iOiI4Iiwid2Vic2l0ZSI6Ikw4IHV0YmlsZG5pbmdzbGljZW5zIiwic3ViIjoiTGl0aXVtIiwicm9sZSI6WyJBZGRPbl9DM1ByaW50Q29ubmVjdG9yIiwiQWRkT25fUHJvZHVjdE1lZGlhTWFwcGVyIl0sIm5iZiI6MTYzNTE1OTgxOSwiZXhwIjoxNjQzNjczNjAwLCJpYXQiOjE2MzUxNTI2MTgsImlzcyI6IkxpY2Vuc2UgU2VydmVyIiwiYXVkIjoiQ049TGljZW5zZVNlcnZlciwgTz1MaXRpdW0ifQ.groaD6tNzJ3j8Ljxd7Tu4M-zIqmmiaAwwapYL9kVimsmA44ZwgnZS93Te9EUgo_xIXsz0LAKg5pV1tnCsB7-pSitux_s96v43tQOJr2tHBln9gwm63lsBNC1bzpPNtAC94LH3nJlAqXLlpJ57eZ-YgrELG4ljBWJ9_hBDFyjHxteScAGAqBaJW2WyvAZDgmIwOGt_eXBm2hb-unduOdIUZUyBGAl1jIxTx5Uts53kUZNbMN3vt5UyXwQn777W97yf6fMcdOBt7yFyfHCMRL0EiZqg0ZhLjDO78LLXkMViz-B-dfVnk7RycpUmt_yIJzUfadqqalV09Pu1PghqcbSyjW-DT3aTwRdqdPl39pAphClKf7thh1-TYfXA9mWSQ_sivwEoQ6EPqka1ol-pDsNdaTgqzWikSM_KNNAqRpDxWB0gBj6I3bVOIhiQOyTMunLYoB70qjzrVKvRImkhGWamGZEkIrcnb_u8ibdgcOobGQ9SCvNy5nH0uJLEVppm5udgPi4yeuplC8eSOJeed5oIv5B2U17lzLuWc3mRkVwz2IxmUScQvdF90bZD1ex7w3QqbwpPHZ5GresHu4AVUspqa_FkY8vrRaO0VVAhlkBX7jklRrpv5LXELKR1PLup-Gp8beS77A7W39tqFNvkIftEtbeSFYqazWLgPFxLfrB4Mk"
ENV Litium__Data__ConnectionString="Pooling=true;User Id=sa;Password=Pass@word;Database=LitiumEducation;Server=host.docker.internal,5434"

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["Src/Litium.Accelerator.Mvc/Litium.Accelerator.Mvc.csproj", "Src/Litium.Accelerator.Mvc/"]
COPY ["Src/Litium.Accelerator.Elasticsearch/Litium.Accelerator.Elasticsearch.csproj", "Src/Litium.Accelerator.Elasticsearch/"]
COPY ["Src/Litium.Accelerator/Litium.Accelerator.csproj", "Src/Litium.Accelerator/"]
COPY ["Src/Litium.Accelerator.FieldTypes/Litium.Accelerator.FieldTypes.csproj", "Src/Litium.Accelerator.FieldTypes/"]
COPY ["Src/Litium.Accelerator.Administration.Extensions/Litium.Accelerator.Administration.Extensions.csproj", "Src/Litium.Accelerator.Administration.Extensions/"]
RUN dotnet restore "Src/Litium.Accelerator.Mvc/Litium.Accelerator.Mvc.csproj"
COPY . .
WORKDIR "/src/Src/Litium.Accelerator.Mvc"
RUN dotnet build "Litium.Accelerator.Mvc.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Litium.Accelerator.Mvc.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Litium.Accelerator.Mvc.dll"]