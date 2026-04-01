import React, { useState } from "react";
import { Input } from "@/components/ui/input";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";

const mockArtists = [
  {
    name: "周杰伦",
    genre: "华语流行",
    bio: "周杰伦（Jay Chou），中国台湾男歌手、音乐人、导演，被誉为华语流行音乐的代表人物之一。他将R&B、嘻哈、古典音乐与中国风元素结合，开创了独特的音乐风格，对华语乐坛影响深远。",
    works: [
      { name: "青花瓷", img: "https://picsum.photos/100?1" },
      { name: "七里香", img: "https://picsum.photos/100?2" },
      { name: "夜曲", img: "https://picsum.photos/100?3" },
      { name: "稻香", img: "https://picsum.photos/100?4" },
    ],
    debut: "2000年",
    image: "https://picsum.photos/400/300?random=1",
  },
  {
    name: "Taylor Swift",
    genre: "Pop",
    bio: "美国创作型歌手，从乡村音乐转型为全球流行巨星，以叙事歌词著称。",
    works: [
      { name: "Love Story", img: "https://picsum.photos/100?5" },
      { name: "Blank Space", img: "https://picsum.photos/100?6" },
    ],
    debut: "2006年",
    image: "https://picsum.photos/400/300?random=2",
  },
];

export default function MusicSearchApp() {
  const [query, setQuery] = useState("");
  const [results, setResults] = useState([]);
  const [selectedArtist, setSelectedArtist] = useState(null);
  const [hasSearched, setHasSearched] = useState(false);

  const handleSearch = () => {
    const filtered = mockArtists.filter((artist) =>
      artist.name.toLowerCase().includes(query.toLowerCase())
    );
    setResults(filtered);
    setHasSearched(true);
  };

  if (selectedArtist) {
    return (
      <div className="min-h-screen bg-gradient-to-br from-pink-100 via-purple-100 to-blue-100 p-6">
        <div className="max-w-3xl mx-auto">
          <Button className="mb-4" onClick={() => setSelectedArtist(null)}>
            ← 返回搜索
          </Button>

          <Card className="rounded-3xl shadow-lg overflow-hidden">
            <img
              src={selectedArtist.image}
              alt={selectedArtist.name}
              className="w-full h-60 object-cover"
            />

            <CardContent className="p-6 space-y-6">
              <div>
                <h1 className="text-3xl font-bold">{selectedArtist.name}</h1>
                <p className="text-gray-500 mt-1">
                  🎵 风格：{selectedArtist.genre} · 📀 出道：{selectedArtist.debut}
                </p>
              </div>

              <div>
                <h2 className="text-lg font-semibold mb-2">👤 艺术家简介</h2>
                <p className="text-gray-700 leading-relaxed">
                  {selectedArtist.bio}
                </p>
              </div>

              {/* 🎶 作品改成带图片卡片 */}
              <div>
                <h2 className="text-lg font-semibold mb-3">🎶 代表作品</h2>
                <div className="grid grid-cols-2 sm:grid-cols-3 gap-4">
                  {selectedArtist.works.map((work, index) => (
                    <div
                      key={index}
                      className="bg-white rounded-xl shadow hover:scale-105 transition p-2"
                    >
                      <img
                        src={work.img}
                        alt={work.name}
                        className="w-full h-24 object-cover rounded-lg mb-2"
                      />
                      <p className="text-sm text-center">{work.name}</p>
                    </div>
                  ))}
                </div>
              </div>
            </CardContent>
          </Card>
        </div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-gradient-to-br from-pink-100 via-purple-100 to-blue-100 p-6">
      <div className="max-w-2xl mx-auto">
        <h1 className="text-3xl font-bold mb-6 text-center">搜索</h1>

        <div className="flex gap-2 mb-4">
          <Input
            placeholder="输入音乐人名字..."
            value={query}
            onChange={(e) => setQuery(e.target.value)}
            onKeyDown={(e) => e.key === "Enter" && handleSearch()}
          />
          <Button onClick={handleSearch}>搜索</Button>
        </div>

        {hasSearched && query && (
          <p className="text-sm text-gray-500 mb-4">
            搜索关键词：<span className="font-semibold text-black">{query}</span>
          </p>
        )}

        <div className="grid gap-4">
          {results.map((artist, index) => (
            <Card
              key={index}
              className="rounded-2xl shadow cursor-pointer hover:scale-[1.02] transition"
              onClick={() => setSelectedArtist(artist)}
            >
              <CardContent className="p-4">
                <h2 className="text-xl font-semibold">{artist.name}</h2>
                <p className="text-gray-500">风格: {artist.genre}</p>
              </CardContent>
            </Card>
          ))}

          {hasSearched && results.length === 0 && (
            <p className="text-gray-400">
              没有找到与 “{query}” 相关的音乐人 🎧
            </p>
          )}
        </div>
      </div>
    </div>
  );
}


